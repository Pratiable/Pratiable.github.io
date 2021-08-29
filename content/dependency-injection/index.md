---
emoji: 💉
title: Dependency Injection
date: '2021-08-29 16:09:38'
author: 이준영
tags: Design Pattern
categories: 디자인패턴
---

> ⛔️ 공부하며 정리한 내용으로 내용상 정확하지 않은 부분이 있을 수 있으니 참고 부탁드립니다!
>
> 수정이 필요한 부분은 Comment 남겨주시면 감사하겠습니다!


# 이 글을 쓰는 목적🤔

`Nest.js`를 공부하다 보니 `Django`를 공부할 때와는 달리 처음보는 개념들이 너무 많았는데 그 중에서 Dependency Injection은 가장 자주나오고 앞으로 무슨 언어 & 프레임워크로 개발을 진행하던지 상관없이 알아야 할 필수 개념, OOP을 위한 부분 같아서 글로써 정리해보고자 한다!

---
# Dependency Injection(의존성 주입)이란?
간단하게 설명하자면 class간의 의존성을 class 외부에서 주입하는것을 말하는데 아래에 하나씩 적어보며 이해해 보기로 하자!

---
## 의존성?
일단 의존성이란 객체들이 서로 의존 관계를 가진 성질이라고 간단하게 정리 할 수 있다!

객체가 두 개가 존재할 때 한 객체가 존재하려면 다른 하나를 필요로 할 때 의존성이 생긴다고 할 수 있는데, 일단
이해가 잘 안되니 아래 코드로 일단 살펴보도록 하겠다!

```typescript
class Dagger {
    attack () :string {
        return 'attacked with a Dagger!'
    }
};

class Rogue {
    private weapon: Dagger;

    constructor() {
        this.weapon = new Dagger();
    }

    public attackMonster() {
        return this.weapon.attack();
    }
}
```

위 코드를 보면 Rogue class는 Dagger class를 내부에서 새로 생성하게 되면서 의존관계가 생기게 되며, weapon을 변경하고 싶어도 다른 weapon으로 변경할 수 없게 되는 문제가 있다!

이런 경우에 `Tight Coupling(강한 결합)`이 일어나게 되는데 이런 경우에 Dagger class를 변경하게 된다면 당연하게도 Rouge class까지 영향을 미치게 된다.

결국 하나의 객체가 수정되었을 때 그 객체를 의존하고 있는 다른 객체까지 수정되어야 하는 문제가 발생하고 Unit Test 작성도 어려워진다.

---
## 주입은?

위에 코드에서는 객체 내부에서 또 다른 객체를 생성해서 사용했는데 외부에서 객체를 생성해서 다른 객체에게 넣어주는 것을 뜻한다.

주입하는 방법은
1. Field Injection
2. Function(Setter) Injection
3. Constructor Injection

세 가지가 있는데 위 세 가지 방법중에 Constructor를 통해서 주입하는 방법으로 진행해 보도록 하겠다.

```typescript
interface Weapon {
    attack(): string;
};

class Dagger implements Weapon {
    attack() {
        return 'attacked with a Dagger!';
    };
};

class Bow implements Weapon {
    attack() {
        return 'attacked with a Bow!';
    };
};

class Rogue {
    private weapon: Weapon;

    constructor(weapon: Weapon) {
        this.weapon = weapon;
    };

    public attackMonster() {
        return this.weapon.attack();
    };
};

const rangedRogue = new Rogue(new Bow);
const meleeRogue = new Rogue(new Dagger);

console.log(rangedRogue.attackMonster())
console.log(meleeRogue.attackMonster())

// "attacked with a Bow!"
// "attacked with a Dagger!" 
```

위 코드를 보면 Rogue객체는 의존하는 type만 알고있고, 매개변수로 정해진 type(예제에서는 Weapon)을 전달하게 되면 어느것이 들어오던지 문제 없이 객체를 생성할 수 있게 된다. 이렇게 코드를 작성하면 의존성이 해소될 수 있다.

하지만 여기서 의존성이 해소되었다고 끝이 아니라 알아야될 한 가지의 개념이 더 존재한다.

---
## Inversion of Control(제어의 역전)
IoC는 객체의 생성이나 Lifecycle의 관리까지 모든 객체에 대한 제어권이 바뀌었다는 것을 의미하는 Design pattern이다.

이것을 구현하기 위해서는 IoC Container가 필요한데 이 역할을 Framework가 해줄 수 있다.

결국 Framework에서 클라이언트 코드에 객체를 주입해서 개발자가 신경써야 할 코드를 줄이는 방법이다.

> IoC Container는 Factory Pattern과 혼동해서 사용할 수 있다고 한다.
> 
> 기본적으로 Factory는 단순히 객체를 생성하게 되지만 IoC Container를 사용 시 IoC의 개념이 적용되어야 한다.
> 
> IoC Container를 사용한다고 해서 그냥 IoC가 일어나는 것이 아니라는 뜻.

Nest.js의 예제를 하나 확인해보자!

```typescript
@Injectable()
export class AccountsService {
    constructor(
        @InjectRepository(Account) private accountRepository: Repository<Account>,
    ) {
    }

    async findAccount(user_account: string): Promise<Account | null> {
        const account = await this.accountRepository.findOne({user_account});

        if (!account) {
            throw new UnauthorizedException('INVALID_ACCOUNT');
        }

        return account;
    }
}
```

위 코드에서 AccountsService의 생성자에 accountRepository 부분을 보게되면 type만 지정해 줬는데도 아래 findAccount부분에서 accountRepository의 findOne이라는 method를 사용하고 있다.

이것이 가능한 이유는 Nest.js에서 accountRepository의 type을 보고 Repository 타입을 알아서 할당해줬기 때문이다.

그래서 개발자는 위 객체의 Lifecycle을 신경쓰지 않아도 편하게 사용하면 되는데 이렇게 생성된 객체는 Nest.js에서 자동으로 관리를 해주기 때문이다.

---

## Dependency Injection의 장단점

지금까지 Dependency Injection에 대해서 여러 예제와 함께 살펴 봤는데 Dependency Injection의 장단점에 대해 짧게 정리해보려고 한다.

### 장점
1. 가독성과 코드의 재사용성을 높여줌
2. 종속성이 감소하기 때문에 변경에 민감하지 않게 됨
3. 객체간의 결합도를 낮춰주기 때문에 유연한 코드작성이 가능해짐
4. 객체들이 분리되어 있어서 Test Case작성 시 효율적임
5. Lifecycle별로 Container를 관리해서 리소스의 낭비를 줄일 수 있음

### 단점
1. DI를 위해서 따로 작업을 해주어야 하기 때문에 간단한 코드를 작성할 땐 번거로울 수 있음
2. 동작, 구성을 분리하기 때문에 코드 추적이 어려울 수 있음

---
## 마치며🎉
DI를 처음 접할 땐 모르는 부분이 많았기 때문에 프로젝트를 진행하며 이게 어떻게 동작하는지 생각할 시간이 너무 없고 작동이 되는 코드만 작성하기에 급급했던 것 같은데
그래도 이번 블로그 글을 정리하면서 DI에 대해 많이 배운 것 같다.

Framework에서 DI를 해준다고 해서 너무 Framework에 의존하지 않고 어떻게 하면 내 코드에서부터 의존성을 줄여 나갈 수 있을까 고민해봐야겠다.

하지만 문제는 이렇게 공부하고 나니 또 모르는 개념들이 쏟아져서 앞으로도 많은 학습이 필요할 것 같은데 그런 부분들도 추가로 블로그 글로 정리해야 할 듯 하다😂

>🛠 _부족한 부분들은 지속적으로 수정 예정!_

---
앞으로 공부해야 할 것들🤙🏻
- Singleton Pattern
- Factory Pattern
- etc....




---
**References**

[출처1](https://medium.com/lemonade-engineering/node-js%EB%A1%9C-%EC%84%A4%EB%AA%85%ED%95%98%EB%8A%94-di-b4a8acf9ce25)

[출처2](https://medium.com/@jang.wangsu/di-dependency-injection-%EC%9D%B4%EB%9E%80-1b12fdefec4f)

[출처3](https://develogs.tistory.com/19)

```toc
```