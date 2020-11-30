# dialog可拖拽,调整宽度

- e-table的表头与内容没对齐（display: table-cell失效）

解决方案1(实测有效):
```html
<style>
.el-table th {
  display: table-cell!important;
}
</style>
```

网上的解决方案2:(在方案1仍不能解决的情况下)
```js
/*重载*/
mounted() {
  setTimeout(()=> {
    this.$refs.queryTable.doLayout();
  },500)
}
```
or 直接加在query后面()
```js
query() {
    query().then(() => {
        this.$refs.queryTable.doLayout();
    })
}
```