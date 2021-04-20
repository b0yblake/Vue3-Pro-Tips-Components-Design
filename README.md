CONTENT
=================

<div id="building-controlled-components"></div><br>

## Chap 1 : BUILD CONTROLLED COMPONENTS

`SANBOX CODE`: [building-controlled-components](https://codesandbox.io/s/oxxlx055xy?from-embed)

- Khi thao tác với `form elements`(input, checkbox, select,..) hãy linh động trong việc xử lý các `$emit` và `props` => hãy tách các `form elements` thành các components riêng lẻ và để trong `global components / common components` <br>
<img src="@img-readme/v-model-tip.jpg" alt="" width="500px" height="auto"><br/>

- Hãy sử dụng `emit gộp` của Vue3 để việc control được hiệu quả hơn
```
// Emit thông thường
this.$emit('myEvent', data)
<my-component @my-event="doSomething"></my-component>

// Emit with Vue3: setup(...)
setup(props, { emit }) { 
  ...
  emit('yourEvent', yourDataIfYouHaveAny);
  // Hoặc sử dụng context(attrs, slots, emit) cho nó ngắn: setup(props, context) {} || context.emit('yourEvent', yourDataIfYouHaveAny);
}
<your-child @yourEvent="onYourEvent" />
onYourEvent(yourDataIfYouHaveAny) {
  ...
}

// Gộp các emit trong 1 setup
methods: {
  isSelected : {
    this.$emit = ('toggle-favorite')
  }
},
emits: {
  'toggle-favorite': function(id) {
    // some condition
  }
}
```

- Sử dụng `v-model` làm tối giản lại phong cách code và nhanh gọn <br>
<img src="@img-readme/v-model-tip.jpg" alt="" width="500px" height="auto"><br/>

- `v-model` có thể sử dụng với các element không thuộc hệ thống `form elements` => với điều kiện chỉ có 1 element duy nhất có trong `component`

```
// Normal with form elements
<input @input="email = $event.target.value">

// Special with element not belong to form
<some-component :value="newLetter" @input="(newValue) => { newLetter = newValue }"></some-component>
```

- Việc lắng nghe liên tục mọi `keydown` || `clicked` khi `end-user` thao tác trên form chỉ thực sự đúng đắn khi làm việc với `reactive form` (form muốn response trực tiếp hành động của user) => hãy tối giản bằng việc khi `submit mới lắng nghe` => giải phóng được 1 phần bộ nhớ và giảm tình trạng lag nếu làm với super form.
```
@input.once
```


<div id="customizing-controlled-component-bindings"></div><br>

## Chap 1 : CUSTOMIZING CONTROLLED COMPONENT BINDINGS

`SANBOX CODE`: [customizing-controlled-component-bindings](https://codesandbox.io/s/mqnzm84plx?from-embed)

- 









































3. [wrapping-external-libraries-as-vue-components](https://codesandbox.io/s/n4qolyr42m?from-embed)
4. [encapsulating-external-behavior-closing-on-escape](https://codesandbox.io/s/1v1o4lvp9j?from-embed)
5. [encapsulating-external-behavior-background-scrolling](https://codesandbox.io/s/z0mx3w9km?from-embed)
6. [encapsulating-external-behavior-portals](https://codesandbox.io/s/vy0k8283o5?from-embed)
7. [encapsulating-external-behavior-reusing-portals](https://codesandbox.io/s/xv1ooy9v1p?from-embed)
8. [injecting-content-using-slots](https://codesandbox.io/s/8x54ow4vl9?from-embed)
9. [native-style-buttons-using-slots-and-class-merging](https://codesandbox.io/s/j4m180n11v?from-embed)
10. [extending-components-using-composition](https://codesandbox.io/s/jj8vjjxlk9?from-embed)
11. [passing-data-up-using-scoped-slots](https://codesandbox.io/s/nwz1xpkyl0?from-embed)
12. [render-functions-101](https://codesandbox.io/s/5vxlz052px?from-embed)
13. [render-functions-and-components](https://codesandbox.io/s/k05o3npx25?from-embed)
14. [render-functions-and-children](https://codesandbox.io/s/7w1pr58p6x?from-embed)
15. [render-functions-and-slots](https://codesandbox.io/s/z2k1j94o8m?from-embed)
16. [data-provider-components](https://codesandbox.io/s/nk9qr8yz0p?from-embed)
17. [getting-started-with-renderless-ui-components](https://codesandbox.io/s/x1z0myl0p?from-embed)
18. [passing-data-props-from-renderless-components](https://codesandbox.io/s/k96ljlz7yv?from-embed)
19. [passing-action-props-from-renderless-components](https://codesandbox.io/s/9l2jwy14mp?from-embed)
20. [passing-binding-props-from-renderless-components](https://codesandbox.io/s/l5yoxyv02q?from-embed)
21. [renderless-ui-components-functions-as-binding-props](https://codesandbox.io/s/kn1nv6ypv?from-embed)
22. [implementing-alternate-layouts-with-renderless-components](https://codesandbox.io/s/1r789z3nnl?from-embed)
23. [configuring-renderless-components](https://codesandbox.io/s/l9v91jn0zq?from-embed)
24. [wrapping-renderless-components](https://codesandbox.io/s/5z5056yoq4?from-embed)
25. [element-queries-as-a-data-provider-component](https://codesandbox.io/s/20r8wnx44r?from-embed)
26. [building-compound-components-with-provide-inject](https://codesandbox.io/s/jl6pz69ox3?from-embed)
27. [building-a-compound-sortable-list-component](https://codesandbox.io/s/o98y1l735y?from-embed)
28. [building-a-search-select-data-bindings](https://codesandbox.io/s/ykypmk03xj?from-embed)
29. [building-a-search-select-filtering](https://codesandbox.io/s/oozwlvk36?from-embed)
30. [building-a-search-select-focus-management](https://codesandbox.io/s/o95oq681l6?from-embed)
31. [building-a-search-select-making-it-controlled](https://codesandbox.io/s/8n0mnm2v70?from-embed)
32. [building-a-search-select-keyboard-navigation](https://codesandbox.io/s/n7mw5871v0?from-embed)
33. [building-a-search-select-click-outside-component](https://codesandbox.io/s/w66mzknr27?from-embed)
34. [building-a-search-select-integrating-popperjs](https://codesandbox.io/s/vyxl1z5pp5?from-embed)
