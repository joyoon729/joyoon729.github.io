---
title: SerializeField 와 HideInspector
date: 2020-07-27 15:04:23
categories: Unity
tags:
  - Unity
---

# Public & Private
Unity 에서는 C# 코드의 Public 과 Private 키워드로 클래스의 Properties 를 유니티 에디터에 노출시킬 지 말지를 결정할 수 있다.

## Public
Public 키워드일 경우, 유니티 에디터에서 해당 변수/메소드를 참조할 수 있다.
```Csharp
public int hp = 100;
```
이 경우 유니티 에디터에서 hp 변수에 값을 지정할 수 있다.

## Private
Private 키워드로 지정할 경우, 유니티 에디터에서 해당 변수/메소드를 참조할 수 없고, 클래스 내에서만 참조 가능하다.
```Csharp
private bool isShow;

public void toggleShow(bool isShow) {
    isShow = !isShow;
}
```
`isShow` 변수는 유니티 에디터에 직접 노출되지 않고, toggleShow 함수를 통해서만 변경 가능하다.

<br>

# SerializeField & HideInspector
## SerializeField
그런데 다른 클래스에서 참조할 수 없도록 private 변수로 만들었는데, 유니티 에디터에서 값을 변경하고 싶은 경우가 있다.
예를 들어, 위의 `isShow` 변수는 **private** 키워드라 클래스 밖에서 변경할 수 없도록 만들었는데, 유니티 에디터에서 처음 기본값을 지정해주고 싶은 경우다.
이럴 땐 `[SerializeField]` 속성을 추가해 private 변수를 유니티 에디터에 노출시킬 수 있다.
```Csharp
[SerializeField]
private bool isShow;
```
이렇게 `[SerializeField]` 속성을 추가하면 클래스 외부에서 참조할 수 없는 private 변수지만 유니티 에디터에서는 접근 가능한 변수가 된다.

## HideInspector
또 반대로, 클래스 외부에서는 참조할 수 있는 public 변수지만, 유니티 에디터에서는 변경할 수 없도록 감추고 싶은 경우도 있다.
이때는 `[HideInspector]` 속성을 추가해 public 변수를 유니티 에디터에서 감출 수 있다.
```Csharp
[HideInspector]
public int hp = 100;
```
이렇게하면 타 클래스에서 접근 가능한 변수이지만, 유니티 에디터에서는 `hp` 변수가 노출되지 않는다.