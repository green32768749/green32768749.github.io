<template>
  <nav id="nav">
    <a
      v-for="(value, key) in $store.state.nav"
      :key="key"
      @click="value.link ? null : go(key)"
      :href="value.link ? value.link : '#'"
      :target="value.link ? '_blank' : null"
      :class="[{ active: $store.state.bookmark === key }, 'icon ' + value.icon]"
      aria-hidden="true"
    >
      <span>{{ value.name }}</span>
    </a>
  </nav>
</template>

<script lang="ts">
import { createComponent } from '@vue/composition-api';
import store from '@/store';

export default createComponent({
  name: 'Nav',
  setup() {
    function go(location: string) {
      store.commit('go', location);
    }
    return { go };
  },
});
</script>

<style lang="scss" scoped>
@import '@/assets/sass/libs/_vendor';
@import '@/assets/sass/breakpoints';

#nav {
  text-align: center;
  height: 4.25em;
  cursor: default;

  a {
    position: relative;
    display: inline-block;
    color: #ffffff;
    width: 1em;
    height: 1em;
    line-height: 0.9em;
    font-size: 2.5em;
    margin: 0 0.25em 0 0.25em;
    opacity: 0.35;
    outline: 0;
    @include vendor('transition', 'opacity .25s ease-in-out');

    &.icon:before {
      padding-right: 0;
    }

    &:before {
      font-size: 0.8em;
    }

    &:after {
      content: '';
      display: block;
      position: absolute;
      left: 50%;
      bottom: -0.75em;
      margin-left: -0.5em;
      border-bottom: solid 0em #ffffff;
      border-left: solid 0.5em transparent;
      border-right: solid 0.5em transparent;
      @include vendor('transition', 'border-bottom-width .25s ease-in-out');
    }

    span {
      display: block;
      position: absolute;
      background: #222222;
      color: #ffffff;
      top: -2.75em;
      font-size: 0.3em;
      height: 2.25em;
      line-height: 2.25em;
      left: 50%;
      opacity: 0;
      @include vendor('transition', 'opacity .25s ease-in-out');
      // Labels not wide enough? Increase its width below and set margin-left to ((width / 2) * -1)
      width: 5.5em;
      margin-left: -2.75em;

      &:after {
        content: '';
        display: block;
        position: absolute;
        bottom: -0.4em;
        left: 50%;
        margin-left: -0.6em;
        border-top: solid 0.6em #222222;
        border-left: solid 0.6em transparent;
        border-right: solid 0.6em transparent;
      }
    }

    &:hover {
      opacity: 1;

      span {
        opacity: 1;
      }
    }

    &.active {
      opacity: 1;

      &:after {
        border-bottom-width: 0.5em;
      }
    }
  }

  @include breakpoint('<=medium') {
    a {
      span {
        display: none;
      }
    }
  }
}
</style>
