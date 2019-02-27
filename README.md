# Nuxt Property Decorator

[![npm](https://img.shields.io/npm/v/nuxt-property-decorator.svg)](https://www.npmjs.com/package/nuxt-property-decorator)

Handy ES / TypeScript decorators for class-style Vue components in Nuxt (based on [Vue class component](https://github.com/vuejs/vue-class-component)) and Property decorators for Vue (bases on [Vue Property Decorator](https://github.com/kaorun343/vue-property-decorator)) and Vuex (based on [Vuex Class](https://github.com/ktsn/vuex-class/)) 

This library fully depends on [vue-class-component](https://github.com/vuejs/vue-class-component).

## License

MIT License

## Install

Installation is very easy 
```bash
npm i -S nuxt-property-decorator
```
or
```bash
yarn add nuxt-property-decorator
```
### Nuxt JS Instructions
Currently decorators need the following two Babel plugins to work   `@babel/plugin-proposal-decorators`,`"@babel/plugin-proposal-class-properties`. Latest Nuxt already adds them for us, the only thing we need is to add `loose` parameter to `@nuxt/babel-preset-app`. Just add this to nuxt-config
```js
    babel: {
      presets({ isServer }) {
        return [
          [
            "@nuxt/babel-preset-app", { loose: true }
          ]
        ]
      }
    }
```

### Nuxt TS Instructions
It works out of the box with Nuxt TS.

## Decorators and helpers

There are following decorators:

* **exported from** `vue-class-component`
  * `@Component` 
* **exported from** `vue-property-decorator` 
  * `@Emit`
  * `@Inject`
  * `@Model`
  * `@Prop`
  * `@Provide`
  * `@Watch`
* **exported from** `vuex-class-component` 
  * `@State`
  * `@Getter`
  * `@Action`
  * `@Mutation`

### Other exports
* `namespace `
* `mixins`
* `Vue`

## Hooks

### Vue Router hooks
  * `beforeRouteEnter`
  * `beforeRouteUpdate`
  * `beforeRouteLeave`

### Nuxt hooks
  * `asyncData`
  * `fetch`
  * `head`
  * `key`
  * `layout`
  * `loading`
  * `middleware`
  * `scrollToTop`
  * `transition`
  * `validate`
  * `watchQuery`
###  Vue-class Hooks
  * `data`
  * `beforeCreate`
  * `created`
  * `beforeMount`
  * `mounted`
  * `beforeDestroy`
  * `destroyed`
  * `beforeUpdate`
  * `updated`
  * `activated`
  * `deactivated`
  * `render`
  * `errorCaptured`
  * `serverPrefetch`
  
## Usage

```typescript
import { Component, Inject, Model, Prop, Vue, Watch } from 'nuxt-property-decorator'

const s = Symbol('baz')

@Component({
  components: { comp }
})
export class MyComponent extends Vue {

  @Inject() foo!: string
  @Inject('bar') bar!: string
  @Inject(s) baz!: string

  @Model('change') checked!: boolean

  @Prop()
  propA!: number

  @Prop({ default: 'default value' })
  propB!: string

  @Prop([String, Boolean])
  propC!: string | boolean

  @Prop({ type: null })
  propD!: any

  @Provide() foo = 'foo'
  @Provide('bar') baz = 'bar'
 
  @Watch('child')
  onChildChanged(val: string, oldVal: string) { }

  @Watch('person', { immediate: true, deep: true })
  onPersonChanged(val: Person, oldVal: Person) { }
  
  beforeRouteLeave (to, from, next) {
    // called when the route that renders this component is about to
    // be navigated away from.
    // has access to `this` component instance.
  }
}

```

is equivalent to

```js
const s = Symbol('baz')

export const MyComponent = Vue.extend({
  name: 'MyComponent',
  components: { comp },
  inject: {
    foo: 'foo',
    bar: 'bar',
    [s]: s
  },
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean,
    propA: Number,
    propB: {
      type: String,
      default: 'default value'
    },
    propC: [String, Boolean],
    propD: { type: null }
  },
  data () {
    return {
      foo: 'foo',
      baz: 'bar'
    }
  },
  provide () {
    return {
      foo: this.foo,
      bar: this.baz
    }
  }
  methods: {
    onChildChanged(val, oldVal) { },
    onPersonChanged(val, oldVal) { }
  },
  beforeRouteLeave (to, from, next) {
    // called when the route that renders this component is about to
    // be navigated away from.
    // has access to `this` component instance.
  },
  watch: {
    'child': {
      handler: 'onChildChanged',
      immediate: false,
      deep: false
    },
    'person': {
      handler: 'onPersonChanged',
      immediate: true,
      deep: true
    }
  }
})
```

As you can see at `propA` and `propB`, the type can be inferred automatically when it's a simple type. For more complex types like enums you do need to specify it specifically.
Also this library needs to have `emitDecoratorMetadata` set to `true` for this to work.

## Useful links

See also:

* [Vue Property Decorator](https://github.com/kaorun343/vue-property-decorator)
* [Vue class component](https://github.com/vuejs/vue-class-component)
* [Vuex Class](https://github.com/ktsn/vuex-class/)
* [Nuxt Class Component](https://github.com/nuxt-community/nuxt-class-component)
