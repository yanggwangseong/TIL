
# ValidationArguments property

## value
> 검증 되고 있는 값 (입력된 값)

## constraints
> 파라미터에 입력된 제한 사항들 (제약 사항을 넣을 수 있는것만)
> @Length를 기준으로 하면 @Length(1, 20)이면 
> `args.constraints[0]`  -> 1    `args.constraints[1]`  -> 20

## targetName
> 검증 하고 있는 클래스의 이름

## object
> 검증하고 있는 객체 (거의 쓸일이 없음)

## property
> 검증 되고 있는 객체의 프로퍼티 이름

