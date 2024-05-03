---
title: "【ReactHookForm ✖️ zod】FormProviderで簡単すっきりフォーム実装"
emoji: "💭"
type: "tech"
topics: [React, TypeScript, Javascript]
published: true
---

ReactHookForm の FormProvider と zod を用いてバリデーションやエラーメッセージの管理などを綺麗に実装していきます。

## スキーマの作成

ここで zod を使用してバリデーションやエラーメッセージを設定していきます。

```tsx
const sampleFormSchema = z.object({
  input: z.string().nonempty({
    message: "inputは必須です",
  }),
  select: z.string().nonempty({
    message: "selectは必須です",
  }),
});
```

## 各フィールドコンポーネントの作成

React Hook Form の useFormContext を使用すると、メソッドを呼び出したりフォーム全体で状態を共有できるようになります。
今回は register メソッドを使って props で受け取った key で form に値を渡していきます。

```tsx
type Props = {
  name: string;
};
function SelectField(props: Props) {
  const methods = useFormContext();
  return (
    <select {...methods.register(props.name)}>
      {["", "one", "two", "three"].map((value) => (
        <option key={value} value={value}>
          {value}
        </option>
      ))}
    </select>
  );
}
```

```tsx
type Props = {
  name: string;
};
function InputField(props: Props) {
  const methods = useFormContext();
  return <input {...methods.register(props.name)} />;
}
```
