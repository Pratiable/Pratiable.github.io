---
emoji: ๐
title: Dependency Injection
date: '2021-08-29 16:09:38'
author: ์ด์ค์
tags: Design Pattern
categories: ๋์์ธํจํด
---

> โ๏ธ ๊ณต๋ถํ๋ฉฐ ์ ๋ฆฌํ ๋ด์ฉ์ผ๋ก ๋ด์ฉ์ ์ ํํ์ง ์์ ๋ถ๋ถ์ด ์์ ์ ์์ผ๋ ์ฐธ๊ณ  ๋ถํ๋๋ฆฝ๋๋ค!
>
> ์์ ์ด ํ์ํ ๋ถ๋ถ์ Comment ๋จ๊ฒจ์ฃผ์๋ฉด ๊ฐ์ฌํ๊ฒ ์ต๋๋ค!


# ์ด ๊ธ์ ์ฐ๋ ๋ชฉ์ ๐ค

`Nest.js`๋ฅผ ๊ณต๋ถํ๋ค ๋ณด๋ `Django`๋ฅผ ๊ณต๋ถํ  ๋์๋ ๋ฌ๋ฆฌ ์ฒ์๋ณด๋ ๊ฐ๋๋ค์ด ๋๋ฌด ๋ง์๋๋ฐ ๊ทธ ์ค์์ Dependency Injection์ ๊ฐ์ฅ ์์ฃผ๋์ค๊ณ  ์์ผ๋ก ๋ฌด์จ ์ธ์ด & ํ๋ ์์ํฌ๋ก ๊ฐ๋ฐ์ ์งํํ๋์ง ์๊ด์์ด ์์์ผ ํ  ํ์ ๊ฐ๋, OOP์ ์ํ ๋ถ๋ถ ๊ฐ์์ ๊ธ๋ก์จ ์ ๋ฆฌํด๋ณด๊ณ ์ ํ๋ค!

---
# Dependency Injection(์์กด์ฑ ์ฃผ์)์ด๋?
๊ฐ๋จํ๊ฒ ์ค๋ชํ์๋ฉด class๊ฐ์ ์์กด์ฑ์ class ์ธ๋ถ์์ ์ฃผ์ํ๋๊ฒ์ ๋งํ๋๋ฐ ์๋์ ํ๋์ฉ ์ ์ด๋ณด๋ฉฐ ์ดํดํด ๋ณด๊ธฐ๋ก ํ์!

---
## ์์กด์ฑ?
์ผ๋จ ์์กด์ฑ์ด๋ ๊ฐ์ฒด๋ค์ด ์๋ก ์์กด ๊ด๊ณ๋ฅผ ๊ฐ์ง ์ฑ์ง์ด๋ผ๊ณ  ๊ฐ๋จํ๊ฒ ์ ๋ฆฌ ํ  ์ ์๋ค!

๊ฐ์ฒด๊ฐ ๋ ๊ฐ๊ฐ ์กด์ฌํ  ๋ ํ ๊ฐ์ฒด๊ฐ ์กด์ฌํ๋ ค๋ฉด ๋ค๋ฅธ ํ๋๋ฅผ ํ์๋ก ํ  ๋ ์์กด์ฑ์ด ์๊ธด๋ค๊ณ  ํ  ์ ์๋๋ฐ, ์ผ๋จ
์ดํด๊ฐ ์ ์๋๋ ์๋ ์ฝ๋๋ก ์ผ๋จ ์ดํด๋ณด๋๋ก ํ๊ฒ ๋ค!

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

์ ์ฝ๋๋ฅผ ๋ณด๋ฉด Rogue class๋ Dagger class๋ฅผ ๋ด๋ถ์์ ์๋ก ์์ฑํ๊ฒ ๋๋ฉด์ ์์กด๊ด๊ณ๊ฐ ์๊ธฐ๊ฒ ๋๋ฉฐ, weapon์ ๋ณ๊ฒฝํ๊ณ  ์ถ์ด๋ ๋ค๋ฅธ weapon์ผ๋ก ๋ณ๊ฒฝํ  ์ ์๊ฒ ๋๋ ๋ฌธ์ ๊ฐ ์๋ค!

์ด๋ฐ ๊ฒฝ์ฐ์ `Tight Coupling(๊ฐํ ๊ฒฐํฉ)`์ด ์ผ์ด๋๊ฒ ๋๋๋ฐ ์ฌ๊ธฐ์ Dagger class๋ฅผ ๋ณ๊ฒฝํ๊ฒ ๋๋ค๋ฉด ๋น์ฐํ๊ฒ๋ Rouge class๊น์ง ์ํฅ์ ๋ฏธ์น๊ฒ ๋๋ค.

๊ฒฐ๊ตญ ํ๋์ ๊ฐ์ฒด๊ฐ ์์ ๋์์ ๋ ๊ทธ ๊ฐ์ฒด๋ฅผ ์์กดํ๊ณ  ์๋ ๋ค๋ฅธ ๊ฐ์ฒด๊น์ง ์์ ๋์ด์ผ ํ๋ ๋ฌธ์ ๊ฐ ๋ฐ์ํ๊ณ  Unit Test ์์ฑ๋ ์ด๋ ค์์ง๋ค.

---
## ์ฃผ์์?

์์ ์ฝ๋์์๋ ๊ฐ์ฒด ๋ด๋ถ์์ ๋ ๋ค๋ฅธ ๊ฐ์ฒด๋ฅผ ์์ฑํด์ ์ฌ์ฉํ๋๋ฐ ์ธ๋ถ์์ ๊ฐ์ฒด๋ฅผ ์์ฑํด์ ๋ค๋ฅธ ๊ฐ์ฒด์๊ฒ ๋ฃ์ด์ฃผ๋ ๊ฒ์ ๋ปํ๋ค.

์ฃผ์ํ๋ ๋ฐฉ๋ฒ์
1. Field Injection
2. Function(Setter) Injection
3. Constructor Injection

์ธ ๊ฐ์ง๊ฐ ์๋๋ฐ ์ ์ธ ๊ฐ์ง ๋ฐฉ๋ฒ์ค์ Constructor๋ฅผ ํตํด์ ์ฃผ์ํ๋ ๋ฐฉ๋ฒ์ผ๋ก ์งํํด ๋ณด๋๋ก ํ๊ฒ ๋ค.

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

์ ์ฝ๋๋ฅผ ๋ณด๋ฉด Rogue๊ฐ์ฒด๋ ์์กดํ๋ type๋ง ์๊ณ ์๊ณ , ๋งค๊ฐ๋ณ์๋ก ์ ํด์ง type(์์ ์์๋ Weapon)์ ์ ๋ฌํ๊ฒ ๋๋ฉด ์ด๋๊ฒ์ด ๋ค์ด์ค๋์ง ๋ฌธ์  ์์ด ๊ฐ์ฒด๋ฅผ ์์ฑํ  ์ ์๊ฒ ๋๋ค. ์ด๋ ๊ฒ ์ฝ๋๋ฅผ ์์ฑํ๋ฉด ์์กด์ฑ์ด ํด์๋  ์ ์๋ค.

ํ์ง๋ง ์ฌ๊ธฐ์ ์์กด์ฑ์ด ํด์๋์๋ค๊ณ  ๋์ด ์๋๋ผ ์์์ผ๋  ํ ๊ฐ์ง์ ๊ฐ๋์ด ๋ ์กด์ฌํ๋ค.

---
## Inversion of Control(์ ์ด์ ์ญ์ )
IoC๋ ๊ฐ์ฒด์ ์์ฑ์ด๋ Lifecycle์ ๊ด๋ฆฌ๊น์ง ๋ชจ๋  ๊ฐ์ฒด์ ๋ํ ์ ์ด๊ถ์ด ๋ฐ๋์๋ค๋ ๊ฒ์ ์๋ฏธํ๋ Design pattern์ด๋ค.

์ด๊ฒ์ ๊ตฌํํ๊ธฐ ์ํด์๋ IoC Container๊ฐ ํ์ํ๋ฐ ์ด ์ญํ ์ Framework๊ฐ ํด์ค ์ ์๋ค.

๊ฒฐ๊ตญ Framework์์ ํด๋ผ์ด์ธํธ ์ฝ๋์ ๊ฐ์ฒด๋ฅผ ์ฃผ์ํด์ ๊ฐ๋ฐ์๊ฐ ์ ๊ฒฝ์จ์ผ ํ  ์ฝ๋๋ฅผ ์ค์ด๋ ๋ฐฉ๋ฒ์ด๋ค.

> IoC Container๋ Factory Pattern๊ณผ ํผ๋ํด์ ์ฌ์ฉํ  ์ ์๋ค๊ณ  ํ๋ค.
> 
> ๊ธฐ๋ณธ์ ์ผ๋ก Factory๋ ๋จ์ํ ๊ฐ์ฒด๋ฅผ ์์ฑํ๊ฒ ๋์ง๋ง IoC Container๋ฅผ ์ฌ์ฉ ์ IoC์ ๊ฐ๋์ด ์ ์ฉ๋์ด์ผ ํ๋ค.
> 
> IoC Container๋ฅผ ์ฌ์ฉํ๋ค๊ณ  ํด์ ๊ทธ๋ฅ IoC๊ฐ ์ผ์ด๋๋ ๊ฒ์ด ์๋๋ผ๋ ๋ป.

Nest.js์ ์์ ๋ฅผ ํ๋ ํ์ธํด๋ณด์!

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

์ ์ฝ๋์์ AccountsService์ ์์ฑ์์ accountRepository ๋ถ๋ถ์ ๋ณด๊ฒ๋๋ฉด type๋ง ์ง์ ํด ์คฌ๋๋ฐ๋ ์๋ findAccount๋ถ๋ถ์์ accountRepository์ findOne์ด๋ผ๋ method๋ฅผ ์ฌ์ฉํ๊ณ  ์๋ค.

์ด๊ฒ์ด ๊ฐ๋ฅํ ์ด์ ๋ Nest.js์์ accountRepository์ type์ ๋ณด๊ณ  Repository ํ์์ ์์์ ํ ๋นํด์คฌ๊ธฐ ๋๋ฌธ์ด๋ค.

๊ทธ๋์ ๊ฐ๋ฐ์๋ ์ ๊ฐ์ฒด์ Lifecycle์ ์ ๊ฒฝ์ฐ์ง ์์๋ ํธํ๊ฒ ์ฌ์ฉํ๋ฉด ๋๋๋ฐ ์ด๋ ๊ฒ ์์ฑ๋ ๊ฐ์ฒด๋ Nest.js์์ ์๋์ผ๋ก ๊ด๋ฆฌ๋ฅผ ํด์ฃผ๊ธฐ ๋๋ฌธ์ด๋ค.

---

## Dependency Injection์ ์ฅ๋จ์ 

์ง๊ธ๊น์ง Dependency Injection์ ๋ํด์ ์ฌ๋ฌ ์์ ์ ํจ๊ป ์ดํด ๋ดค๋๋ฐ Dependency Injection์ ์ฅ๋จ์ ์ ๋ํด ์งง๊ฒ ์ ๋ฆฌํด๋ณด๋ ค๊ณ  ํ๋ค.

### ์ฅ์ 
1. ๊ฐ๋์ฑ๊ณผ ์ฝ๋์ ์ฌ์ฌ์ฉ์ฑ์ ๋์ฌ์ค
2. ์ข์์ฑ์ด ๊ฐ์ํ๊ธฐ ๋๋ฌธ์ ๋ณ๊ฒฝ์ ๋ฏผ๊ฐํ์ง ์๊ฒ ๋จ
3. ๊ฐ์ฒด๊ฐ์ ๊ฒฐํฉ๋๋ฅผ ๋ฎ์ถฐ์ฃผ๊ธฐ ๋๋ฌธ์ ์ ์ฐํ ์ฝ๋์์ฑ์ด ๊ฐ๋ฅํด์ง
4. ๊ฐ์ฒด๋ค์ด ๋ถ๋ฆฌ๋์ด ์์ด์ Test Case์์ฑ ์ ํจ์จ์ ์
5. Lifecycle๋ณ๋ก Container๋ฅผ ๊ด๋ฆฌํด์ ๋ฆฌ์์ค์ ๋ญ๋น๋ฅผ ์ค์ผ ์ ์์

### ๋จ์ 
1. DI๋ฅผ ์ํด์ ๋ฐ๋ก ์์์ ํด์ฃผ์ด์ผ ํ๊ธฐ ๋๋ฌธ์ ๊ฐ๋จํ ์ฝ๋๋ฅผ ์์ฑํ  ๋ ๋ฒ๊ฑฐ๋ก์ธ ์ ์์
2. ๋์, ๊ตฌ์ฑ์ ๋ถ๋ฆฌํ๊ธฐ ๋๋ฌธ์ ์ฝ๋ ์ถ์ ์ด ์ด๋ ค์ธ ์ ์์

---
## ๋ง์น๋ฉฐ๐
DI๋ฅผ ์ฒ์ ์ ํ  ๋ ๋ชจ๋ฅด๋ ๋ถ๋ถ์ด ๋ง์๊ธฐ ๋๋ฌธ์ ํ๋ก์ ํธ๋ฅผ ์งํํ๋ฉฐ ์ด๊ฒ ์ด๋ป๊ฒ ๋์ํ๋์ง ์๊ฐํ  ์๊ฐ์ด ๋๋ฌด ์๊ณ  ์๋์ด ๋๋ ์ฝ๋๋ง ์์ฑํ๊ธฐ์ ๊ธ๊ธํ๋ ๊ฒ ๊ฐ์๋ฐ
๊ทธ๋๋ ์ด๋ฒ ๋ธ๋ก๊ทธ ๊ธ์ ์ ๋ฆฌํ๋ฉด์ DI์ ๋ํด ๋ง์ด ๋ฐฐ์ด ๊ฒ ๊ฐ๋ค.

Framework์์ DI๋ฅผ ํด์ค๋ค๊ณ  ํด์ ๋๋ฌด Framework์ ์์กดํ์ง ์๊ณ  ์ด๋ป๊ฒ ํ๋ฉด ๋ด ์ฝ๋์์๋ถํฐ ์์กด์ฑ์ ์ค์ฌ ๋๊ฐ ์ ์์๊น ๊ณ ๋ฏผํด๋ด์ผ๊ฒ ๋ค.

ํ์ง๋ง ๋ฌธ์ ๋ ์ด๋ ๊ฒ ๊ณต๋ถํ๊ณ  ๋๋ ๋ ๋ชจ๋ฅด๋ ๊ฐ๋๋ค์ด ์์์ ธ์ ์์ผ๋ก๋ ๋ง์ ํ์ต์ด ํ์ํ  ๊ฒ ๊ฐ์๋ฐ ๊ทธ๋ฐ ๋ถ๋ถ๋ค๋ ์ถ๊ฐ๋ก ๋ธ๋ก๊ทธ ๊ธ๋ก ์ ๋ฆฌํด์ผ ํ  ๋ฏ ํ๋ค๐

>๐  _๋ถ์กฑํ ๋ถ๋ถ๋ค์ ์ง์์ ์ผ๋ก ์์  ์์ !_

---
์์ผ๋ก ๊ณต๋ถํด์ผ ํ  ๊ฒ๋ค๐ค๐ป
- Singleton Pattern
- Factory Pattern
- etc....




---
**References**

[์ถ์ฒ1](https://medium.com/lemonade-engineering/node-js%EB%A1%9C-%EC%84%A4%EB%AA%85%ED%95%98%EB%8A%94-di-b4a8acf9ce25)

[์ถ์ฒ2](https://medium.com/@jang.wangsu/di-dependency-injection-%EC%9D%B4%EB%9E%80-1b12fdefec4f)

[์ถ์ฒ3](https://develogs.tistory.com/19)

```toc
```