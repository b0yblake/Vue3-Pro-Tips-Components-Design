CONTENT
=================

<div id="building-controlled-components"></div><br>

## Chap 1 : BUILD CONTROLLED COMPONENTS

`SANBOX CODE`: [building-controlled-components](https://codesandbox.io/s/oxxlx055xy?from-embed)

NOTE: <br>
✔ Bản chất của `v-model` là việc lắng nghe `input`(:modelValue="pageTitle") và emit dữ liệu(@update:modelValue="pageTitle = $event") <br>
✔ `v-model` chỉ nên dùng cho `input` và `component` => không nên sử dụng cho các thành phần khác <br>
✔ Sử dụng `emit` và `gộp các emit` <br>
✔ `form` tối ưu bằng cách loại bỏ real-time sync in input `@input.once` => chỉ check khi click submit <br>
✔ Sử dụng `Object.fromEntries(new FormData(event.target))` thay cho `v-model` nếu dùng với form nhiều thành phần <br>

- Khi thao tác với `form elements`(input, checkbox, select,..) hãy linh động trong việc xử lý các `$emit` và `props` => hãy tách các `form elements` thành các components riêng lẻ và để trong `global components / common components` <br>
<img src="@img-readme/simple-form.jpg" alt="" width="500px" height="auto"><br/>

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
<ChildComponent v-model="pageTitle" />

export default {
  props: {
    modelValue: String // previously was `value: String`
  },
  emits: ['update:modelValue'],
  methods: {
    changePageTitle(title) {
      this.$emit('update:modelValue', title) // previously was `this.$emit('input', title)`
    }
  }
}
```

- Sử dụng `v-model` làm tối giản lại phong cách code và nhanh gọn <br>
Vue 2: <br>
<img src="@img-readme/v-model-tip.jpg" alt="" width="500px" height="auto"><br/>

```
Vue 3 thay đổi syntax của v-model:

<ChildComponent v-model="pageTitle" />

<!-- shorthand for: -->
<ChildComponent
  :modelValue="pageTitle"
  @update:modelValue="pageTitle = $event"
/>

```

- `v-model` có thể sử dụng với các element không thuộc hệ thống `form elements` => với điều kiện chỉ có 1 element duy nhất có trong `component`

```
// Normal with form elements
<input @input="email = $event.target.value">

// Special with element not belong to form
<some-component :modelValue="newLetter" @update:modelValue="(newValue) => { newLetter = newValue }"></some-component>
```

- Việc lắng nghe liên tục mọi `keydown` || `clicked` khi `end-user` thao tác trên form chỉ thực sự đúng đắn khi làm việc với `reactive form` (form muốn response trực tiếp hành động của user) => hãy tối giản bằng việc khi `submit mới lắng nghe` => giải phóng được 1 phần bộ nhớ và giảm tình trạng lag nếu làm với super form.
```
@input.once
```


<div id="customizing-controlled-component-bindings"></div><br>

## Chap 2 : CUSTOMIZING CONTROLLED COMPONENT BINDINGS

`SANBOX CODE`: [customizing-controlled-component-bindings](https://codesandbox.io/s/mqnzm84plx?from-embed)

NOTE: <br>
✔ `v-model` default sẽ không được tự nhiên vì sử dụng `modelValue` & `update:modelValue` => có thể custom bằng các value cụ thể <br>

```
//Parent
<Custom-component v-model:message="message" />

//Child
<input type="text" :value="message" @input='emit("update:message", $event.target.value);'>
```
## Chap 3 : WRAPPING EXTERNAL LIBRARIES AS VUE COMPONENTS

`SANBOX CODE`: [wrapping-external-libraries-as-vue-components](https://codesandbox.io/s/n4qolyr42m?from-embed)

NOTE: <br>
✔ Hãy chú ý sử dụng các method có sẵn của lib để custom lại các event <br>

- Đôi khi, việc thêm 1 thư viện ngoài(datePicker) vào để gọi trong input gây phiền toái nhất định: $event chọn ngày không cập nhật với biến ref thông thường. Vì chúng không được tạo ra để sync với biến đó => hãy sử dụng các custom event của lib (onSelect()) để xác định event click day. <br>
- https://github.com/b0yblake/Vue3-Form-Best-Practice/blob/main/src/components/common/form/DatepickerPikaday.vue

## Chap 4 : ENCAPSULATING EXTERNAL BEHAVIOR CLOSING ON ESCAPE

`SANBOX CODE`: [encapsulating-external-behavior-closing-on-escape](https://codesandbox.io/s/1v1o4lvp9j?from-embed)

NOTE: <br>
✔ `Web accessibility` luôn luôn đặt lên hàng đầu mỗi khi thao tác với dialog <br>
✔ Dialog khi bật lên cần được kiểm soát cả ở phần `keyboard`: `close = esc, enter, blankspace` <br>
✔ Hành vi người dùng cần được chú trọng khi họ dùng `keyboard` <br>
✔ Đối với những DOM sinh ra sau `lifecycle:created`, nếu muốn control được, nên sử dụng `nextTick` hoặc sử dụng vanila js tại thời điểm click <br>
✔ Hãy linh động trong việc sử dụng js, chú ý đến việc tối ưu hiệu suất (dùng js tối ưu được hiệu suất ngay tại component do không phải `v-model` 2way-binding) <br>

```
Way1: sử dụng keydown = esc button, enable tabindex để có thể focus được 
@keydown.esc="handleEsc" tabindex="0" ref="dialog"

Way2: sử dụng vanila js nhằm bắt sự kiện click tại thời điểm dialog đã sinh ra (không cần care về việc sinh ra hay sau dom update)

=======================
methods: {
  createClickEvent: function() {
    let self = this;

    document
      .querySelector(".util_per_pay_rate .btn_open_dialog")
      .addEventListener("click", function() {
        self.dialog = true;
      });
  }
},
mounted() {
  this.createClickEvent();
}
========================
created() {
  document.addEventListener('keydown', (e) => {
    if(e.key === 'Escape' && this.show) {
      this.createClickEvent();
    }
  })
}
```

## Chap 5 : ENCAPSULATING EXTERNAL BEHAVIOR BACKGROUND SCROLLING

`SANBOX CODE`: [encapsulating-external-behavior-background-scrolling](https://codesandbox.io/s/z0mx3w9km?from-embed)

NOTE: <br>
✔ Khi bật `Dialog` vấn đề gặp phải là chúng ta cần remove scroll: hãy chú ý về cách sử dụng bằng class toggle tại body => chúng sẽ hữu hiệu khi chúng ta handle được, còn không => hãy sử dụng vanila js <br>
✔ Hãy cố gắng cover 1-2 case tiếp theo sau này khi mở rộng app <br>

```
watchEffect(() => {
  props.active ? props.preventBackgroundScrolling && document.body.style.setProperty('overflow', 'hidden') : props.preventBackgroundScrolling && document.body.style.setProperty('overflow')
})
```

## Chap 6 : ENCAPSULATING EXTERNAL BEHAVIOR PORTALS

`SANBOX CODE`: [encapsulating-external-behavior-portals](https://codesandbox.io/s/vy0k8283o5?from-embed)

NOTE: <br>
✔ Với các `dialog` trên từng component, hãy cứ viết ở trên các components để dễ handle data, sau đó sử dụng `teleport` kết hợp với `slot` <br>
✔ Việc handle data của tất cả các popup ở cùng 1 component trung gian đem lại hiệu quả rõ rệt so với việc handle data tại các component common <br>
https://github.com/b0yblake/Vue3-Form-Best-Practice/blob/main/src/views/Form.vue <br>

```
//index.html
<body>
  <div id="app"></div>
  <!-- Use teleport to move dialog to here -->
  <div id="layer"></div>
</body>

// Component
<teleport to="#layer">
  <some-component-dialog :data="data" @click="some-element" />
</teleport>
```

## Chap 7 : ENCAPSULATING EXTERNAL RESING PORTALS

`SANBOX CODE`: [encapsulating-external-behavior-reusing-portals](https://codesandbox.io/s/xv1ooy9v1p?from-embed)

NOTE: <br>
✔ 

































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
