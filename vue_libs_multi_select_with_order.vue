<template>
    <div class="multi-select-with-order" v-bind:class="{'all-items-in-use': allItemsInUse, 'hidden-list': hiddenList}">

        <div class="multi-select-items" v-if="showMultiSelectItems">
            <div v-for="item in selectValue" class="multi-select-item" v-bind:data-item="item">
                <span class="item-text">{{ item }}</span>
                <div class="close">&times;</div>
            </div>
        </div>

        <span class="add-button">+</span>
        <select>
            <option></option>
            <option v-bind:value="item" v-for="item in itemsNotInUse">{{ item }}</option>
        </select>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                selectValue: this.value,
                hiddenList: this.hiddenList,
                showMultiSelectItems: true
            };
        },
        mounted: function () {
            this.hiddenList = true;

            const self = this;
            const selectEl = $(this.$el).children('select');

            function initItemsContainer() {
                const multiSelectItems = $(self.$el).children('.multi-select-items');

                multiSelectItems.disableSelection();

                multiSelectItems.delegate('.close', 'click', function (event, myName, myValue) {
                    self.selectValue.splice($(this).parent().index(), 1);
                    self.$emit('input', self.selectValue);
                });

                const sortableContainer = $(self.$el).children('.multi-select-items');

                sortableContainer.sortable({
                    update: function(event, ui) {
                        self.selectValue = ui.item.parent()
                            .children('.multi-select-item')
                            .map(function() {return $(this).attr('data-item')})
                            .get();
                        self.showMultiSelectItems = false;
                        setTimeout(function() {
                            self.showMultiSelectItems = true;
                            setTimeout(function() {
                                self.$emit('input', self.selectValue);
                                initItemsContainer();
                            }, 0);
                        }, 0);
                    }
                });
            }
            initItemsContainer();

            function createSelect2Element() {
                selectEl
                    .select2()
                    .on('change', function (evt) {
                        const newValue = selectEl.select2('val');
                        if (!newValue) {
                            return;
                        }
                        self.selectValue.push(newValue);
                        self.$emit('input', self.selectValue);
                        selectEl.val(null).trigger('change');
                        self.hiddenList = true;
                        setTimeout(function() {
                            selectEl.select2('destroy');
                            setTimeout(createSelect2Element, 0)
                        }, 0)
                    });
            }
            createSelect2Element();

            $(this.$el).children('.add-button')
                .click(function () {
                    self.hiddenList = false;
                    setTimeout(function() {
                        selectEl.select2('open');
                        $('.select2-input').on( "blur", function() {
                            self.hiddenList = true;
                        });
                    }, 0);
                });
        },
        props: [
            'value',
            'items'
        ],
        watch: {
            value: function (val) {
                this.selectValue = val;
            }
        },
        methods: {},
        components: {},
        computed: {
            itemsNotInUse: function () {
                return this.items.filter(item => this.selectValue.indexOf(item) == -1);
            },
            allItemsInUse: function () {
                return this.itemsNotInUse.length === 0;
            }
        }
    }
</script>

<style>
    @import url('//cdnjs.cloudflare.com/ajax/libs/select2/4.0.3/css/select2.min.css');
    .multi-select-with-order {
        border: 1px solid #dbdbdb;
        border-radius: 4px;
        padding: 10px;
    }
    .multi-select-with-order .add-button {
        font-size: 28px;
        font-weight: bold;
        vertical-align: middle;
        cursor: pointer;
        display: none;
        vertical-align: super;
    }
    .multi-select-with-order .multi-select-items {
        display: inline-block;
    }
    .multi-select-with-order .multi-select-items .multi-select-item {
        vertical-align: bottom;
        display: inline-block;
        border: 1px solid #dbdbdb;
        border-radius: 4px;
        padding: 4px;
        margin: 4px;
        background-color: white;
        cursor: pointer;
    }
    .multi-select-with-order .multi-select-items .multi-select-item .close {
        display: inline-block;
        border-left: 1px solid #dbdbdb;
        cursor: pointer;
        padding: 0 1px 0 4px;
        font-weight: bold;
        font-size: 20px;
    }
    .multi-select-with-order .multi-select-items .multi-select-item .item-text {
        padding-right: 4px;
    }
    .multi-select-with-order .select2-container {
        margin: 8px;
        width: 200px;
    }
    .multi-select-with-order.hidden-list .select2-container {
        display: none;
    }
    .multi-select-with-order.hidden-list .add-button {
        display: inline;
    }
    .multi-select-with-order.all-items-in-use .select2-container {
        display: none;
    }
    .multi-select-with-order.all-items-in-use .add-button {
        display: none;
    }
</style>