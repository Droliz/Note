## 在rust中获取到document

```rust
use wasm_bindgen::UnwrapThrowExt;
use web_sys::window;

let document = window()
    .expect_throw("window is undefined")
    .document()
    .expect_throw("document is undefined");
```

