<template>
  <div class="field">
    <!-- ラベルの文字列が指定された時だけ表示 -->
    <label class="label" v-if="label">{{ label }}</label>
    <!-- iconが指定された時だけ必要なクラスを設定 -->
    <!-- v-model="value" は:valueと@inputの組み合わせ -->
    <p class="control" :class="{ 'has-icons-left': icon }">
      <input
        class="input"
        :class="{ 'is-danger': error }"
        :type="type"
        :placeholder="placeholder"
        @change="onChanged"
        :value="value"
        @input="input"
      />
      <!-- アイコンが指定されたときだけ表示 -->
      <pm-icon v-if="icon" class="is-small is-left" :name="icon"></pm-icon>
    </p>
    <p class="help is-danger" v-if="error">{{ error }}</p>
  </div>
</template>

<script>
import pmIcon from "./Icon";

export default {
  name: "pm-text-field",
  components: { pmIcon },
  props: {
    // type: String;で省略できる
    // propsのtypeオプションは受け取るデータの型の指定
    type: {
      type: String,
      label: String,
      //親コンポーネントから指定されなかった場合にdefaultで入るもの
      // ここではinputタグなので<input type="text">のように展開される
      default: "text",
      // typeの属性値がvalidatorで指定したもの以外を受け取るとconsoleにエラーを出す
      validator(val) {
        return ["text", "email", "password", "search", "url"].includes(val);
      }
    },
    placeholder: String,
    value: String,
    icon: String,
    error: String
  },
  // computed: {
  //   innerValue: {
  //     get() {
  //       return this.value;
  //     },
  //     // v-modelで更新があればinputイベントを親に発行し変更してもらう
  //     set(newValue) {
  //       this.$emit("input", newValue);
  //     }
  //   }
  methods: {
    input(e) {
      if (e.target.value !== this.value) {
        this.$emit("input", e.target.value);
      }
    },
    onChanged(e) {
      // changeイベントを発火し入力値をパラメータとして送信
      this.$emit("change", e.target.value);
    }
  }
};
</script>
