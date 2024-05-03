---
title: "
【ReactHookForm ✖️ zod】
FormProviderで簡単すっきりフォーム実装
"
emoji: "💭"
type: "tech"
topics: [React, TypeScript, Javascript]
published: true
---

ReactHookForm の FormProvider と zod を用いてバリデーションやエラーメッセージの管理など実装していきます。

## 使用するライブラリ

https://www.npmjs.com/package/zod
https://www.npmjs.com/package/react-hook-form
https://www.npmjs.com/package/@hookform/resolvers

## FormProvider の実装

```tsx
import { useForm, FormProvider } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";

function App() {
  const methods = useForm({
    //zodResolverでスキーマを指定
    resolver: zodResolver(sampleFormSchema),
  });
  const { handleSubmit } = methods;
  return (
    <FormProvider {...methods}>
      <form onSubmit={handleSubmit((data) => console.log(data))}>
        <InputField name='input' />
        <SelectField name='select' />
        <button type='submit'>Submit</button>
      </form>
    </FormProvider>
  );
}
```

## スキーマの作成

ここで zod を使用してバリデーションやエラーメッセージを設定していきます。

```tsx
import { z } from "zod";

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
import { useFormContext } from "react-hook-form";

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
import { useFormContext } from "react-hook-form";

type Props = {
  name: string;
};
function InputField(props: Props) {
  const methods = useFormContext();
  return <input {...methods.register(props.name)} />;
}
```
