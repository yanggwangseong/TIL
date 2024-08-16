> request 및 response 개체를 액세스 할 수 있으며, Express 미들웨어와 동일하다.
> next 메소드 호출하여 다음 미들웨어 기능으로 제어를 전달

![[Pasted image 20240310214930.png]]

- middleware 클래스 작성

```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log('Request...');
    next();
  }
}
```

> constructor 안에서 의존성을 주입(DI) 할 수 있다.


- 해당 미들웨어 적용방법

```typescript
import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';
import { LoggerMiddleware } from './common/middleware/logger.middleware';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes('cats');
  }
}
```


## forRoutes
> 해당 Middleware를 적용 할 경로나 클래스

```typescript
import { Module, NestModule, RequestMethod, MiddlewareConsumer } from '@nestjs/common';
import { LoggerMiddleware } from './common/middleware/logger.middleware';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes({ path: 'cats', method: RequestMethod.GET });
  }
}
// .forRoutes(CatsController);
```

## exlcude
> 해당 Middleware에서 제외 할 경로

```typescript
consumer
  .apply(LoggerMiddleware)
  .exclude(
    { path: 'cats', method: RequestMethod.GET },
    { path: 'cats', method: RequestMethod.POST },
    'cats/(.*)',
  )
  .forRoutes(CatsController);
```


## 다중 미들웨어
```typescript
consumer.apply(cors(), helmet(), logger).forRoutes(CatsController);
```


- 미들웨어 마다 경로 설정해주기

```typescript
@Module({
controllers: [CatsController],
})
export class AppModule implements NestModule {
		configure(consumer: MiddlewareConsumer) {
		consumer
		.apply(Logger1Middleware)
		.forRoutes({ path: 'cats', method: RequestMethod.GET });
		
		consumer
		.apply(Logger2Middleware)
		.forRoutes({ path: 'cats', method: RequestMethod.POST });
		
		consumer
		.apply(Logger3Middleware)
		.forRoutes({ path: 'cats/:id', method: RequestMethod.ALL });
	}
}
```


## 미들웨어 사용시 주의 할점
```ts
export class FeedsModule implements NestModule {
		configure(consumer: MiddlewareConsumer) {
		consumer.apply(FeedExistsMiddleware).forRoutes('*');
	}
}
```

> 모든 경로로 받게끔 지정 했을때

```ts
@Injectable()

export class FeedExistsMiddleware implements NestMiddleware {
	use(req: Request, res: Response, next: NextFunction) {
		console.log('******params=*******', req.params);
		next();
	}
}
```

> params를 출력 해보면 `******params=******* { '0': 'feeds/a4706fd6-85b2-478b-8f42-d48cbf54fe54' }`
> 이런식으로 출력된다.

```ts
// 방법1
@Injectable()
export class FeedsModule implements NestModule {
		configure(consumer: MiddlewareConsumer) {
		consumer.apply(FeedExistsMiddleware).forRoutes(FeedsController);
	}
}

// 방법2
@Injectable()
export class FeedsModule implements NestModule {
		configure(consumer: MiddlewareConsumer) {
		consumer.apply(FeedExistsMiddleware).forRoutes('feeds/:feedId');
	}
}
```

> 해당 방법들로 사용 해야 정상적으로 path value를 가져올 수 있다.


### req.params가 undefined일때 값
```ts
const feedId = req.params.feedId;
@Injectable()

export class FeedExistsMiddleware implements NestMiddleware {
constructor(private readonly feedsRepository: FeedsRepository) {}
async use(req: Request, res: Response, next: NextFunction) {

...
```

> 만약 req.params.feedId 가 undefined이면
> undefined가 나오는게 아니라 :feedId가 나온다.
> 좀 치명적인것 같은데 좀 더 safe하게 사용하기 위해 joi를 그냥 사용해야겠다.


```ts
@Injectable()

export class FeedExistsMiddleware implements NestMiddleware {

constructor(private readonly feedsRepository: FeedsRepository) {}

async use(req: Request, res: Response, next: NextFunction) {

const feedId = req.params.feedId;
if (!feedId) throw EntityNotFoundException(ERROR_FEED_NOT_FOUND);

  

const schema = Joi.string().guid({
	version: ['uuidv4'],
});

  

const { error } = schema.validate(feedId);

  

if (error) {
	throw EntityNotFoundException(ERROR_FEED_NOT_FOUND);
}

  

const feed = await this.feedsRepository.findFeedById(feedId);

  

if (!feed) {
	throw EntityNotFoundException(ERROR_FEED_NOT_FOUND);
}

next();
}

}
```



