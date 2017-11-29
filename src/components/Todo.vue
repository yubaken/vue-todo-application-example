<template>
  <div>
    <p>
    <span class="single-article">
      更新時刻： {{updateAt | dateFormat}}
    </span>
    </p>
    <div class="grid header single-article">
      <div class="item"></div>
      <div class="item">
        Id
        <i v-if="upSort === 'id'" class="fa fa-caret-up" @click="sortId"></i>
        <i v-else class="fa fa-caret-down" @click="sortId"></i>
      </div>
      <div class="item">
        Todo
      </div>
      <div class="item">
        更新日時
        <i v-if="upSort === 'updateAt'" class="fa fa-caret-up" @click="sortUpdateAt"></i>
        <i v-else class="fa fa-caret-down" @click="sortUpdateAt"></i>
      </div>
    </div>
    <template v-for="item in items">
      <div class="grid single-article">
        <!--fromに値を渡す場合、v-modelにすると変更も自動的に検知してくれる-->
        <div class="item"><input type="checkbox" v-model="item.checked"/></div>
        <div class="item">{{item.id}}</div>
        <div class="item"><input type="text" style="width: 100%" v-model="item.todo"/></div>
        <!--filtersで定義したdateFormat関数の結果が表示される-->
        <div class="item">{{item.updateAt | dateFormat}}</div>
      </div>
    </template>
  </div>
</template>

<script>
  export default {
    name: 'Todo',
    // propsに指定されたキーがhtmlタグの属性として受け取れるようになる
    // 値は this.{キーの名前} で受け取れる
    // ただし、template内はthis不要
    props: ['items', 'updateAt'],
    filters: { // https://jp.vuejs.org/v2/guide/filters.html
      dateFormat: function (date) {
        if (!date) return '';
        return date.toLocaleDateString("ja-JP", {
          weekday: "long", year: "numeric", month: "short",
          day: "numeric", hour: "2-digit", minute: "2-digit", second: "2-digit"
        })
      }
    },
    data () {
      return {
        upSort: ''
      }
    },
    methods: { // https://jp.vuejs.org/v2/guide/events.html
      sortId: function () {
        if(this.upSort && this.upSort === 'id') {
          this.upSort = '';
        } else {
          this.upSort = 'id';
        }

        const self = this;

        // NG 親コンポーネントから受け取った値は直接変更してはいけない
        // このコンポーネントは変更されたことが分かるが、渡し元である親コンポーネントが変更を検知出来なくなる
        // https://jp.vuejs.org/v2/guide/components.html#sync-%E4%BF%AE%E9%A3%BE%E5%AD%90
        // https://jp.vuejs.org/v2/guide/components.html#%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%A0%E3%82%A4%E3%83%99%E3%83%B3%E3%83%88
        // this.updateAt = new Date();

        // OK
        this.$emit('update:updateAt', new Date());

        // Array.prototype.map
        // https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/map
        // currentValue: 現在処理中の配列内の要素。
        // index: 現在処理中の配列内の要素のインデックス。
        // array: map を実行している配列。
        const tmpItems = this.items.map(function (currentValue, index, array) {
          return array[array.length - index - 1]
        });

        // NG 親コンポーネントから受け取った値は直接変更してはいけない
        // 配列はemitで変更を行えないのでVue.setを使用する
        // https://jp.vuejs.org/v2/guide/list.html#%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AE%E5%A4%89%E6%9B%B4%E6%A4%9C%E5%87%BA%E3%81%AE%E6%B3%A8%E6%84%8F
        // this.items = tmpItems;

        // OK
        tmpItems.forEach(function (currentValue, index, array) {
          self.$set(self.items, index, currentValue);
        });
      },
      sortUpdateAt: function () {
        if(this.upSort && this.upSort === 'updateAt') {
          this.upSort = '';
        } else {
          this.upSort = 'updateAt';
        }

        // NG
        this.updateAt = new Date();
      }
    }
  }
</script>

<style scoped>
  .grid {
    display: grid;
    grid-template-columns: 50px 60px 2fr 1.2fr;
  }

  .header {
    font-weight: 800;
  }

  .item{
    padding: 0 10px;
  }

  .single-article {
    text-align: left;
    background: white;
    -webkit-transition: opacity .35s ease-in;
    transition: opacity .35s ease-in;
    border: 1px solid #bfc5c9;
    box-shadow: 0px 3px 9px #bdc5cd;
    border-radius: 6px;
    width: 94%;
    margin: auto;
    padding: 10px 4px;
    position: relative;
  }
</style>
