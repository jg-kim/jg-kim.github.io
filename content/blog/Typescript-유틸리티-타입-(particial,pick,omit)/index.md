---
title: Typescript 유틸리티 타입
date: "2023-02-01T09:43:37.121Z"
---

## 유틸리티 타입
>Typescript는 일반적인 타입을 변환하기 용이하게 몇 가지 유틸리티 타입을 제공

꼭 쓰지 않아도 되지만 잘 정의한 타입으로 중복 없이 유용하게 사용할 수 있음

### #`Partial<Type>`

---

* 특정 타입의 부분 집합을 만족하는 타입을 정의
```typescript
interface Todo {
  title: string;
  description: string;
}
 
function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
  return { ...todo, ...fieldsToUpdate };
}
 
const todo1 = {
  title: "organize desk",
  description: "clear clutter",
};
 
// Todo type의 description 부분만 사용
const todo2 = updateTodo(todo1, {
  description: "throw out trash",
});
```

### #`Pick<Type, Keys>`

---

* 특정 속성만 추출하여 타입을 정의
```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}
 
type TodoPreview = Pick<Todo, "title" | "completed">;
 
const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};
```

### #`Omit<Type, Keys>`

---

* 특정 속성을 **제외**하여 타입을 정의
```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
  createdAt: number;
}
 
// description 만 제외한 title, completed, createdAt
type TodoPreview = Omit<Todo, "description">;
 
const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
  createdAt: 1615544252770,
};
```

