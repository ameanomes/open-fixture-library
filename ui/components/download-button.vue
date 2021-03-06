<template>
  <div :class="{'download-button': true, 'big': big}">
    <a href="#" class="title" @click.prevent>{{ title }}</a>
    <ul>
      <li v-for="plugin in exportPlugins" :key="plugin.key">
        <a
          :href="`${baseLink}.${plugin.key}`"
          :title="`Download ${plugin.name} fixture definition${isSingleFixture ? `` : `s`}`"
          rel="nofollow"
          @click="blur($event)">
          {{ plugin.name }}
        </a>
      </li>
    </ul>
  </div>
</template>

<style lang="scss">
.download-button {
  & > .title {
    display: block;
    box-sizing: border-box;
    width: 100%;
    padding: 0.5ex 2ex;
    border-radius: 2px;
    background: $orange-500;
    font-weight: 600;
    color: $primary-text-light;
    cursor: pointer;
    box-shadow: 0 2px 2px rgba(#000, 0.2);
    transition: border-radius 0.2s, background-color 0.2s;

    /* down arrow */
    &::before {
      content: '';
      display: block;
      width: 0;
      height: 0;
      border-width: 0.4em 0.4em 0;
      border-style: solid;
      border-color: currentcolor transparent transparent;
      float: right;
      margin: 0.8em 0 0 1ex;
    }
  }

  & > ul {
    position: absolute;
    left: -9999px;
    top: 100%;
    padding: 0.7em 0;
    margin: 0;
    width: 100%;
    list-style: none;
    background-color: $grey-50;
    border-radius: 0 0 2px 2px;
    box-shadow: 0 2px 2px rgba(#000, 0.2);
    z-index: 90;

    & a {
      display: block;
      padding: 0.2ex 2ex;
      color: $primary-text-dark;
      transition: background-color 0.2s;
    }

    & a:hover,
    & a:focus {
      background-color: $grey-200;
      outline: 0;
    }
  }

  &:hover > .title,
  & > .title:focus,
  & > .title:active {
    border-radius: 2px 2px 0 0;
    background: $orange-700;
  }

  &:hover > ul,
  & > .title:focus + ul,
  & > .title:active + ul {
    left: 0;
  }

  /* single rule since unsupporting browsers skip the whole rule */
  &:focus-within > .title {
    border-radius: 2px 2px 0 0;
    background: $orange-700;
  }
  &:focus-within > ul {
    left: 0;
  }
}

.fixture-header > .download-button {
  display: block;
  position: relative;
  margin: 0 0 1em;
}

/* move download button to the right */
@media (min-width: 650px) {
  .fixture-header {
    display: -ms-flexbox;
    display: flex;
    -ms-flex-direction: row;
    flex-direction: row;
    align-items: flex-start;

    & > .title {
      -ms-flex: 1 1 auto;
      flex-grow: 1;
      flex-shrink: 1;
    }

    & > .download-button {
      -ms-flex: 0 0 auto;
      flex-grow: 0;
      flex-shrink: 0;
      margin: 1.5rem 0 0;
      width: 14em;

      &.big {
        width: 19em;

        & .title {
          font-size: 1.1em;
        }
      }
    }
  }
}
</style>

<script>
import plugins from '~~/plugins/plugins.json';

export default {
  props: {
    download: {
      // either the fixture key or the number of fixtures
      type: [String, Number],
      required: true
    },
    big: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  data() {
    return {
      exportPlugins: plugins.exportPlugins.map(
        pluginKey => ({
          key: pluginKey,
          name: plugins.data[pluginKey].name
        })
      )
    };
  },
  computed: {
    isSingleFixture() {
      return typeof this.download === `string`;
    },
    title() {
      if (this.isSingleFixture) {
        return `Download as …`;
      }

      return `Download all ${this.download} fixtures`;
    },
    baseLink() {
      if (this.isSingleFixture) {
        return `/${this.download}`;
      }

      return `/download`;
    }
  },
  methods: {
    blur(event) {
      event.target.blur();
    }
  }
};
</script>
