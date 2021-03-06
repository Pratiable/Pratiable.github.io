---
emoji: ๐งช
title: Jest๋ก Nest.js ์ ๋ํ์คํธ ํด๋ณด๊ธฐ!
date: '2021-09-25 18:41:08'
author: ์ด์ค์
tags: unit-test, jest
categories: Testing
---

## Nest.js์์์ unit-test

Nest.js๋ ๊ธฐ๋ณธ ํ์คํธ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ก jest๋ฅผ ์ง์ํ๊ณ  ์๋ค. CLI๋ก ์๋น์ค๋ ์ปจํธ๋กค๋ฌ๋ฅผ ์์ฑํ  ๋ ํ์ผ๋ช ๋ค์ `spec.ts`๊ฐ ๋ถ์ ํ์ผ์ด ํ์คํธ๋ฅผ ์ํด ์๋์ผ๋ก ์์ฑ๋๋ค.

```js
import { Test, TestingModule } from '@nestjs/testing';
import { UsersService } from './users.service';

describe('UsersService', () => {
  let service: UsersService;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [UsersService],
    }).compile();

    service = module.get < UsersService > UsersService;
  });

  it('should be defined', () => {
    expect(service).toBeDefined();
  });
});
```

> spec.ts๊ฐ ์์ฑ๋์์ ๋ ๊ธฐ๋ณธ ์ฝ๋

---

## Jest๋?

์ผ๋จ Nest.js์ unit-test๋ฅผ ์์ฑํ๊ธฐ ์ ์ Jest์ ๋ํด์ ์์๋ณผ ํ์๊ฐ ์๋ค๊ณ  ์๊ฐํ๋ค.

Jest๋ Facebook์์ ๋ง๋  ํ์คํธ ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ด๊ณ  Jest๊ฐ ๋์ค๊ธฐ ์ด์ ์ javascript ์ฝ๋๋ฅผ ํ์คํธํ๊ธฐ ์ํด์๋ ์ฌ๋ฌ ๊ฐ์ง ํ์คํธ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ค์ ์ค์นํด์ ์๋ก ์กฐํฉํด์ ์ฌ์ฉํด์ผ ํ๋๋ฐ Jest๋ฅผ ์ฌ์ฉํ  ๊ฒฝ์ฐ์๋ ๋ง์ ๊ธฐ๋ฅ์ ํ ๋ฒ์ ์ง์ํ๊ธฐ ๋๋ฌธ์ ํจ๊ณผ์ ์ธ ํ์คํธ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ผ๊ณ  ํ  ์ ์๋ค.

ํ์คํธ ์ฝ๋๊ฐ ์ง๊ด์ ์ด๊ธฐ๋ ํ๊ณ  ๋ฌธ์ํ๋ ์ ๋์ด์์ด ์์ฐ์ฑ, ๊ฐ๋์ฑ ๋ฉด์์๋ ํจ๊ณผ์ ์ด๋ค!

---

## Nest.js์์์ Jest๋ฅผ ํ์ฉํ Mocking

์ด์  Nest.js์์์ Jest๋ฅผ ํ์ฉํ Mocking์ ์งํํด ๋ณผ ํ๋ฐ ๊ทธ ์ ์ ์ผ๋จ Mocking์ด ๋ญ์ง ๊ฐ๋จํ๊ฒ ์ ๋ฆฌํด๋ณด๊ฒ ๋ค.

### Mocking?

Mocking์ ๋จ์ test๋ฅผ ์์ฑํ  ๋ ํด๋น ์ฝ๋๊ฐ ์์กดํ๊ณ  ์๋ ๋ถ๋ถ์ ๊ฐ์ง๋ก ๋์ฒดํ๋ ๊ธฐ๋ฒ์ ๋งํ๋ค. ์ผ๋ฐ์ ์ผ๋ก ํ์คํธํ  ์ฝ๋๊ฐ ์์กดํ๋ ๋ถ๋ถ์ ์ง์  ์์ฑํ๊ธฐ ์ด๋ ค์ด ๊ฒฝ์ฐ mocking์ด ๋ง์ด ํ์ฉ๋๋ค!

Mocking์ ํ์ง ์๊ณ  ์ค์  ํ๊ฒฝ๊ณผ ๊ฐ์ด ํ์คํธ ํ๊ฒฝ์ ์ค์ ํ๊ฒ ๋์ ๋ ์ค์  db๋ฅผ ์ฌ์ฉํ๋ค๋ฉด ์ฌ๋ฌ ๋ฌธ์ ์ ์ด ๋ฐ์๋  ์ ์๋๋ฐ

- db ์ ์๊ณผ ๊ฐ์ด network์ด๋ I/O ์์์ด ํฌํจ๋ ํ์คํธ๋ ์คํ ์๋๊ฐ ๋๋ ค์ง
- CI/CD์ ์ผ๋ถ์ ํ์คํธ๊ฐ ์๋ํ๋์ด ์๋ค๋ฉด ์๋๊ฐ ๋๋ ค์ ๋ฌธ์ ๊ฐ ์๊ธธ ์ ์์
- ํ์คํธ ์์ฒด๋ฅผ ์ํ ์ฝ๋๋ณด๋ค db์ ์ฐ๊ฒฐํ๊ณ  ํธ๋์ญ์ ์์ฑ, ์ฟผ๋ฆฌ ์ ์กํ๋ ์ฝ๋๊ฐ ๋ ๊ธธ์ด์ง ์ ์์
- ํ์คํธ ์คํ ์๊ฐ db๊ฐ ์คํ๋ผ์ธ ๋๋ค๋ฉด ํ์คํธ๋ ์คํจํ๊ณ  ํ์คํธ ์์ฒด๊ฐ ์ธํ๋ผ ํ๊ฒฝ์ ์ํฅ์ ๋ฐ์

์์ ๊ฐ์ ๋ฌธ์  ๋ง๊ณ ๋ ์ฌ๋ฌ ๊ฐ์ง ํฌ๊ณ  ์์ ๋ฌธ์ ๋ค์ด ๋ฐ์ํ  ์ ์๊ณ , ํน์  ๊ธฐ๋ฅ๋ง ๋ถ๋ฆฌํด์ ํ์คํธํ๊ฒ ๋ค๋ **unit test์ ๋ชฉ์ ์ ๋ถํฉํ์ง ์๊ฒ**๋๋ค.

Mocking์ ์ด๋ฐ ์ํฉ์์ ์ค์  ๊ฐ์ฒด์ฒ๋ผ ์ฌ์ฉ ๊ฐ๋ฅํ ๊ฐ์ง ๊ฐ์ฒด๋ฅผ ์์ฑํ๋ ๋ฉ์ปค๋์ฆ ์ ๊ณต ๋ฐ ํ์คํธ ์คํ ์ค ๊ฐ์ง ๊ฐ์ฒด์ ์ด๋ค ์ผ๋ค์ด ๋ฐ์ํ๋์ง ๊ธฐ์ตํ๊ณ  ์๊ธฐ ๋๋ฌธ์ ๋ด๋ถ์ ์ผ๋ก ์ด ๊ฐ์ฒด๊ฐ ์ด๋ป๊ฒ ์ฌ์ฉ๋๋์ง ๊ฒ์ฆ์ด ๊ฐ๋ฅํ๋ค.

---

### Mocking์ผ๋ก ํ์คํธ ํ๊ฒฝ ์ค๋น

Mocking์ด ์ด๋ค ๊ฑด์ง ๊ฐ๋จํ๊ฒ ์์๋ดค์ผ๋ ์ค์  jest์์ ์ด๋ป๊ฒ mocking์ผ๋ก unit-test ํ๊ฒฝ์ ์ค๋นํ ์ง ์ฝ๋๋ก ํ์ธํด ๋ณด์!

```ts
const mockUserRepository = {
  save: jest.fn(),
  findOne: jest.fn(),
};

/* 
MockRepository๋ฅผ type alias๋ก ์ ์ํ๊ณ  Partial, Record๋ฑ์ ์ ํธ๋ฆฌํฐ ํด๋์ค๋ฅผ ์ฌ์ฉํ์ฌ repository๋ฅผ mockingํ  ์ค๋น๋ฅผ ํ๋ค.
Record๋ฅผ ์ฌ์ฉํ์ฌ Repository<T>์ ๋ฉ์๋๋ฅผ ์ถ์ถํ ๊ฐ์ key๊ฐ ํ์์ผ๋ก, jest.Mock์ value๊ฐ์ผ๋ก ๊ฐ๋ ํ์์ ๋ฆฌํดํ๋ค.
๊ทธ๋ฆฌ๊ณ  Repository์ ๋ฉ์๋๋ฅผ ์ ๋ถ ์ฌ์ฉํ  ๊ฒ์ด ์๋๊ธฐ ๋๋ฌธ์ Partial์ ์ฌ์ฉํด์ ๋ฉ์๋๋ฅผ optionalํ๊ฒ ๊ฐ์ ธ์๋ค.
*/
type MockRepository<T = any> = Partial<Record<keyof Repository<T>, jest.Mock>>;

describe('UsersService', () => {
  let service: UsersService;
  let userRepository: MockRepository<User>;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        UsersService,
        { provide: getRepositoryToken(User), useValue: mockUserRepository }, //custom provider
      ],
    }).compile();

    service = module.get<UsersService>(UsersService);
    userRepository = module.get<MockRepository<User>>(getRepositoryToken(User));
  });

  it('should be defined', () => {
    expect(service).toBeDefined();
  });
});
```

- `MockRepository`๋ฅผ ํ์ฉํ์ฌ `Repository`๋ฅผ Mocking
- ์ฌ๊ธฐ์ mockingํ  Repository๋ ํ๋์ด๊ธฐ ๋๋ฌธ์ `mockUserRepository`๋ฅผ object ํํ๋ก ์์ฑํด๋์ง๋ง ์ฌ๋ฌ repository๋ฅผ mockingํ๋ ๊ฒฝ์ฐ์๋ ํจ์๋ก ์์ฑํด ์ฃผ๋ ๊ฒ ์ข๋ค.
- `mockUserRepository`์์ ํ์ฌ ์ฌ์ฉํ  ๋ฉ์๋๋ `save`์ `findOne`์ด๊ธฐ ๋๋ฌธ์ ๋ ๋ฉ์๋๋ฅผ `jest.fn`์ ์ฌ์ฉํด์ ๋ฉ์๋๋ฅผ mockingํด์คฌ๋ค.
- `getRepositoryToken`์ custom provider๋ก ์ฌ์ฉํ๋๋ฐ `@InjectRepository()`๋ฅผ ์ฌ์ฉํ์ฌ `UsersRepository`๋ฅผ ์์ฒญํ  ๋๋ง๋ค `useValue`๋ก ๋ฑ๋ก๋ `mockUserRepository`๊ฐ์ฒด๋ฅผ ์ฌ์ฉํ๊ฒ ๋๋ค.

์์ฒ๋ผ ์ค์ ํด๋๋ฉด mocking์ ์๋ฃ๋๊ณ  ํ์คํธ ์ฝ๋๋ฅผ ์์ฑํ  ๋ db์ ์ง์  ์ ์ํ๊ฒ ๋๋ ๊ฒ์ด ์๋๋ผ mockingํ repository๋ฅผ ํธ์ถํ๊ฒ ๋๋ค.

---

### unit-test ์์ฑํด๋ณด๊ธฐ

๊ทธ๋ผ ์ ์ ๋ฅผ ์์ฑํ๋ ํจ์๋ฅผ ํ์คํธํ๋ ์ฝ๋๋ฅผ ์์ฑํด๋ณด์!

```ts
describe('createUser', () => {
  const createUserArguments = {
    email: 'test@test.co.kr',
    password: 'rhrlrkr1234',
    name: 'tester',
    phoneNumber: '010-1234-1234',
  };

  it('should create user', async () => {
    userRepository.findOne.mockResolvedValue(undefined);
    userRepository.save.mockResolvedValue(createUserArguments);

    const result = await service.createUser(createUserArguments);

    expect(userRepository.save).toHaveBeenCalledTimes(1);
    expect(userRepository.save).toHaveBeenCalledWith(createUserArguments);

    expect(result).toEqual({ success: true });
  });
});
```

- `createUserArguments`๋ถ๋ถ์ ์ ์  ์์ฑ์ ์ํด์ ๋ฏธ๋ฆฌ ์์ฑํด๋ arguments์ด๊ณ  ์๋์์ mockingํ ํจ์๋ฅผ ์ฌ์ฉํ  ๋ ์ด ๊ฐ์ ๋ฃ์ด์ ํธ์ถํ๋ฉด ๋๋ค.
- `mockResolvedValue`๋ promise๋ฅผ ๋ฐํํ  ๋ ์ฌ์ฉํ๋๋ฐ `save`๋ `promise<User>`๋ฅผ ๋ฐํํ๊ธฐ ๋๋ฌธ์ ์ธ์๋ก `createUserArguments`๋ฅผ ์ค์ ํด ์คฌ๋ค.
- `toHaveBeenCalledTimes`๋ฅผ ์ฌ์ฉํ๊ฒ ๋๋ค๋ฉด ํด๋น method๊ฐ ๋ช ๋ฒ ํธ์ถ๋์๋์ง ์ฒดํฌํ  ์ ์๋ค.
- `toHaveBeenCalledWith`์ ์์ ๋น์ทํ๊ฒ ์ด๋ค ์ธ์๋ฅผ ๋ฐ์๋์ง ์ฒดํฌํ  ์ ์๋ค.
- ๋ง์ง๋ง์ผ๋ก createUser์ return๊ฐ์ด ์ ์์ ์ผ๋ก ๋์ค๋์ง ์ฒดํฌํ๋ค.

---

### unit-test๊ฐ ์ ์ ์๋ํ๋์ง ํ์ธํ๊ธฐ

ํ์ฌ ๋ด๊ฐ ํ์คํธํ  ๋ถ๋ถ์ user์ service๋ด ๋ก์ง์ ํ์คํธํ  ๊ฒ์ด๊ธฐ ๋๋ฌธ์ `npm run test user.service`๋ช๋ น์ด๋ฅผ ํฐ๋ฏธ๋์ ์คํํ๊ฒ ๋๋ฉด `user.service`์ ํ์คํธํ  ์ ์๊ฒ ๋๋ค.

๋ช๋ น์ด๋ฅผ ์คํํ๊ณ  ๊ธฐ๋ค๋ฆฌ๊ฒ ๋๋ฉด!!!!

![test-result](./createUser-unit-test-result.png)

์ด๋ ๊ฒ ์ ์์ ์ผ๋ก ํ์คํธ๊ฐ ์๋ฃ๋ ๊ฒ์ ๋ณผ ์ ์๋ค!

๋ง์ฝ ํ์คํธ๊ฐ ์คํจํ๊ฒ ๋๋ ๊ฒฝ์ฐ์๋๐ฟ

![test-fail-result](./createUser-unit-test-fail-result.png)

์ด๋ ๊ฒ ์ด๋ ํ์คํธ์์ ์คํจํ๋์ง, ์ด๋ค ๋ก์ง์์ ๋ค๋ฅธ์ง ๋ฑ๋ฑ ์์ธํ๊ฒ ํ์ํด ์ค์ ์ด๋ค ๊ฒ์ด ๋ฌธ์ ์ธ์ง ๋น ๋ฅด๊ฒ ํ์ํ  ์ ์๋ค.

---

### ๋ง์น๋ฉฐ

Jest๋ฅผ ์ฌ์ฉํ๋ฉด unit-test๋ฅผ ํ์คํ ์ฝ๊ฒ ํ  ์ ์๋ ์ฅ์ ์ด ์๋ค. ํ์ฌ ์์ ์ ์งํํ๋ ํ๋ก์ ํธ๋ฅผ django์์ nest.js๋ก ๋ง์ด๊ทธ๋ ์ด์ ํ๊ณ  ์๋๋ฐ unit-test๋ฅผ ํ์คํ๊ฒ ์์ฑํ๋ฉฐ ์งํํ๋ ค๊ณ  ํ๋ค ๋ณด๋ ์ธํด์ญ์ ์งํํ  ๋๋ ๋ณด์ง ๋ชปํ๋ ๋ํ์ผํ ํจ์์ ์ฌ์ฉ๋ฒ ๋ฑ์ ๋ค์ ํ์ธํ๊ฒ ๋์๋ค. ์์ง ์ด๋ฒ ๊ธ์์ ์๊ฐํ์ง ๋ชปํ jest์ ์ฌ๋ฌ ๊ธฐ๋ฅ๋ค์ด ์กด์ฌํ๋๋ฐ ๊ทธ๋ฐ ๋ถ๋ถ๋ค์ ์์ผ๋ก ์ฝ๋ ์์ฑ์ ํ๋ฉด์ ๋ธ๋ก๊ทธ์ ๋ค๋ฅธ ๊ธ๋ก ์ ๋ฆฌํด ๋ณผ ๊ฒ์ด๋ค!

์ ๋ณด์ ์๋ชป๋ ๋ถ๋ถ์ด ์๋ค๋ฉด ๋๊ธ๋ก ํผ๋๋ฐฑ ์ฃผ์๋ฉด ํ์ธ ํ ์์ ํ๋๋ก ํ๊ฒ ์ต๋๋ค!๐

<br>
<br>
<br>
<br>
<br>

---

**ref.**

[์ถ์ฒ 1](https://www.daleseo.com/jest-fn-spy-on/)
[์ถ์ฒ 2](https://darrengwon.tistory.com/1004?category=915252)

```toc

```
