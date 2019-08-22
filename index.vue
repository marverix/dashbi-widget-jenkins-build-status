<template>
  <div class="jenkins-build-state"
       :data-state="state">
    <h3>{{ title }}</h3>

    <i :class="[icon]"></i>
    <div class="build-state">{{ state }}</div>

    <div class="build-id">#{{ id }}</div>
    <div class="build-duration">{{ duration }}</div>
  </div>
</template>

<script>

const iconMap = {
  running: 'fa-cog fa-spin',
  aborted: 'fa-hand-paper-o',
  failure: 'fa-exclamation',
  'not-built': 'fa-question',
  success: 'fa-check',
  unstable: 'fa-exclamation-triangle'
};

export default {

  // Props
  props: {
    title: {
      type: String,
      required: true
    },

    data: {
      type: Array,
      required: true
    },

    params: {
      default: null
    }
  },

  // Computed
  computed: {

    /**
     * Last state
     * @returns {Object|null}
     */
    lastState () {
      return this.data.length ? this.data[this.data.length - 1].state : null;
    },

    /**
     * Id
     * @returns {number|null}
     */
    id () {
      return this.lastState ? this.lastState.id : null;
    },

    /**
     * Duration
     * @returns {string|null}
     */
    duration () {
      return this.lastState ? Date.toHmsFormat(this.lastState.duration) : null;
    },

    /**
     * Result
     * @returns {string|null}
     */
    result () {
      return this.lastState ? this.lastState.result : null;
    },

    /**
     * State
     * @returns {}
     */
    state () {
      /**
        null        running
        ABORTED     aborted     The build was manually aborted
        FAILURE     failure     The build had a fatal error
        NOT_BUILT   not-built   The module was not built
        SUCCESS     success     The build had no errors
        UNSTABLE    unstable    The build had some errors but they were not fatal
      */
      return typeof this.result === 'string' ? this.result.toKebabCase() : 'running';
    },

    /**
     * Icon classes
     * @returns {string}
     */
    icon () {
      return `fa fa-fw ${iconMap[this.state]}`;
    }

  }
}
</script>

<style lang="less" scoped>
@import '~@less/colors.less';

@state-running: @blue;
@state-aborted: @darker;
@state-failure: @red;
@state-not-built: @yellow;
@state-success: @green;
@state-unstable: @purple;

.jenkins-build-state {
  width: auto;
  height: 100%;
  color: white;
  padding: 0 0.5em;
  position: relative;

  &[data-state='running'] {
    background-color: @state-running;
  }

  &[data-state='aborted'] {
    background-color: @state-aborted;
  }

  &[data-state='failure'] {
    background-color: @state-failure;
  }

  &[data-state='not-built'] {
    background-color: @state-not-built;
  }

  &[data-state='success'] {
    background-color: @state-success;
  }

  &[data-state='unstable'] {
    background-color: @state-unstable;
  }

  h3 {
    height: 3.5em;
    overflow: hidden;
  }

  i.fa {
    font-size: 4em;
  }

  .build-state {
    font-size: 2em;
  }

  .build-id {
    position: absolute;
    bottom: 0.5em;
    left: 1em;
    opacity: 0.5;
  }

  .build-duration {
    position: absolute;
    bottom: 0.5em;
    right: 1em;
    opacity: 0.5;
  }
}

</style>
