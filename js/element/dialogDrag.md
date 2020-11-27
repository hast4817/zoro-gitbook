# dialog可拖拽,调整宽度

- element-ui dialog组件添加可拖拽位置 可拖拽宽高

>原文见(https://github.com/Ray-56/m-dream/issues/8)

我稍微做了优化并提供了示例

示例:
```vue
    <!--添加v-dialogDrag 支持拖拽; 添加v-dialogDragWidth支持修改宽度 -->
    <el-dialog title="批量编辑" :visible.sync="thisDialog" width="60%" :close-on-click-modal="false"
               v-dialogDrag ref="dialog__wrapper">
      <div class="line" v-dialogDragWidth="$refs.dialog__wrapper">
        <el-table :data="dialogData" stripe border>
            ...
        </el-table>
        <div slot="footer" class="dialog-footer">
          <el-button @click="closeDialog()">取 消</el-button>
          <el-button type="primary" @click="editConfirm()">确定修改</el-button>
        </div>
      </div>
    </el-dialog>
```

引用js文件
```js
import 'common/js/dialogDrag.js'
```

js文件具体内容如下
```js
import Vue from 'element'

// v-dialogDrag: 弹窗拖拽
Vue.directive('dialogDrag', {
    bind(el, binding, vnode, oldVnode) {
        const dialogHeaderEl = el.querySelector('.el-dialog__header')
        const dragDom = el.querySelector('.el-dialog')
        dialogHeaderEl.style.cursor = 'move'

        // 获取原有属性 ie dom元素.currentStyle 火狐谷歌 window.getComputedStyle(dom元素, null);
        const sty = dragDom.currentStyle || window.getComputedStyle(dragDom, null)

        dialogHeaderEl.onmousedown = (e) => {
            // 鼠标按下，计算当前元素距离可视区的距离
            const disX = e.clientX - dialogHeaderEl.offsetLeft
            const disY = e.clientY - dialogHeaderEl.offsetTop

            // 获取到的值带px 正则匹配替换
            let styL, styT

            // 注意在ie中 第一次获取到的值为组件自带50% 移动之后赋值为px
            if (sty.left.includes('%')) {
                styL = +document.body.clientWidth * (+sty.left.replace(/%/g, '') / 100)
                styT = +document.body.clientHeight * (+sty.top.replace(/%/g, '') / 100)
            } else {
                styL = +sty.left.replace(/\px/g, '')
                styT = +sty.top.replace(/\px/g, '')
            }

            document.onmousemove = function (e) {
                // 通过事件委托，计算移动的距离
                const l = e.clientX - disX
                const t = e.clientY - disY

                // 移动当前元素
                dragDom.style.left = `${l + styL}px`
                dragDom.style.top = `${t + styT}px`

                // 将此时的位置传出去
                // binding.value({x:e.pageX,y:e.pageY})
            }

            document.onmouseup = function (e) {
                document.onmousemove = null
                document.onmouseup = null
            }
        }
    }
})

// v-dialogDragWidth: 弹窗宽度拖大 拖小
Vue.directive('dialogDragWidth', {
    bind(el, binding, vnode, oldVnode) {
        const dragDom = binding.value.$el.querySelector('.el-dialog');
        const lineElPosition = ['right', 'left'];
        for (let i in lineElPosition) {
            const lineEl = document.createElement('div');
            lineEl.style =
                'width: 2px; background: inherit; height: 80%; position: absolute; ' + lineElPosition[i] + ': 0; top: 0; bottom: 0; margin: auto; z-index: 1; cursor: w-resize;';
            lineEl.addEventListener(
                'mousedown',
                function (e) {
                    // 鼠标按下，计算当前元素距离可视区的距离
                    const disX = e.clientX - el.offsetLeft;

                    // 当前宽度
                    const curWidth = dragDom.offsetWidth;

                    document.onmousemove = function (e) {
                        e.preventDefault(); // 移动时禁用默认事件

                        // 通过事件委托，计算移动的距离
                        const l = e.clientX - disX;

                        if (lineElPosition[i] === 'right') {
                            dragDom.style.width = `${curWidth + l}px`;
                        } else {
                            dragDom.style.width = `${curWidth - l}px`;
                        }
                    };

                    document.onmouseup = function (e) {
                        document.onmousemove = null;
                        document.onmouseup = null;
                    };
                },
                false
            );
            dragDom.appendChild(lineEl);
        }
    }
})
```