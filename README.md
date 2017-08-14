# Vue Simple Multi-Select With Order

Simple multi-select component with order - you can not only manage a set of items (like a regular multi-select) but also their order.

No sass/less compilation. Very easy to use :).

<img src="https://raw.githubusercontent.com/AlexLibs/vue-libs-multi-select-with-order/master/demo/vue-libs-multi-select-with-order.png" /><br>

## Installation
Add the dependencies to your html file:
```
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/jqueryui/1.11.4/jquery-ui.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jqueryui-touch-punch/0.2.3/jquery.ui.touch-punch.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/select2/3.4.8/select2.min.js"></script>
```

```bash
npm install vue-libs-multi-select-with-order --save
```

## Basic Usage

```javascript
import MultiSelectWithOrder from 'vue-libs-multi-select-with-order';

new Vue({

    components: {
        MultiSelectWithOrder
    },

    data () {
        return {
            keys: ['white', 'red', 'blue'],
            availableKeys: ['orange', 'yellow', 'green', 'white', 'red', 'blue', 'black', 'brown']
        }
    }
};
```

```html
            <multi-select-with-order class="ordered-keys" v-model="keys"
                                     v-bind:items="availableKeys">
            </multi-select-with-order>
```

## Customization

Override the relevant css classes to customize it.