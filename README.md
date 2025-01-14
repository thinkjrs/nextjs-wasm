This is a [Next.js](https://nextjs.org) project bootstrapped with [`create-next-app`](https://nextjs.org/docs/app/api-reference/cli/create-next-app) which uses a Rust function compiled into a WebAssembly module to calculate Fibonacci numbers.

## Getting Started

First, run the development server:

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can examine how this project defines, loads and uses the WebAssembly module within `src/components/Fibonacci.tsx`.

## Basic outline

Using [`wasm-bindgen`](https://rustwasm.github.io/docs/wasm-bindgen/) with [`wasm-pack`](https://rustwasm.github.io/docs/wasm-pack/) we have a simple and maintanable way to pull in
functions we write in Rust into our Next.js apps using the app router.

Here's the basic gist:

1. Create Rust crate: `cargo new fibonacci --lib`

- `fibonacci/`

2. Add two things to your `Cargo.toml`:

```toml
[dependencies]
wasm-bindgen = "0.2"

[lib]
crate-type = ["cdylib"]
```

3. Add your Rust library code
4. Compile Rust into WebAssembly: `wasm-pack build --target web --out-dir ../src/app/pkg`
   > _Note: this was executed from within the Rust crate._
5. Update Next.js eslint configuration:

```ts
const eslintConfig = [
  ...compat.extends("next/core-web-vitals", "next/typescript"),
  // WASM-specific rule adjustments
  {
    files: ["src/app/pkg/fibonacci.js"],
    rules: {
      "@typescript-eslint/no-unused-vars": "off",
    },
  },
];
```

6. Add Next.js component using the Rust WebAssembly function, must be a client component with the `"use client"` directive.

- Import the WebAssembly `init` function and types
- Define the `WebAssembly` object
- Initialize the function
- Call the function

7. Render the component in your Next.js app.

## Learn More

To learn more about Next.js, Rust, and WebAssembly take a look at the following resources:

- [My blog post on the topic - thinkjrs.dev](https://thinkjrs.dev/blog/rust-and-nextjs-with-webassembly?ref=github-nextjs-wasm)
- [The Rust book](https://doc.rust-lang.org/book/)
- [Rust By Example](https://doc.rust-lang.org/rust-by-example/)
- [wasm-bindgen](https://rustwasm.github.io/docs/wasm-bindgen/)
- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.
- [WebAssembly](https://webassembly.org/)
- [@mpadge's deeper dive - Next.js & WebAssembly](https://github.com/mpadge/wasm-next)

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js) - your feedback and contributions are welcome!

## Deploying

This app is [deployed on Vercel](). You can do the same by forking the repo and connecting that in your Vercel account.

Check out the [Next.js deployment documentation](https://nextjs.org/docs/app/building-your-application/deploying) for more details on how to do that.
