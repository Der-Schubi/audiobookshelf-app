<template>
  <div class="relative w-full" v-click-outside="clickedOutside">
    <p class="text-sm font-semibold" :class="disabled ? 'text-fg-muted' : ''">{{ label }}</p>
    <button type="button" :disabled="disabled" class="relative w-full border border-border rounded shadow-sm pl-3 pr-8 py-2 text-left focus:outline-none text-sm" :class="buttonClass" aria-haspopup="listbox" aria-expanded="true" @click.stop.prevent="clickShowMenu">
      <span class="flex items-center" :class="!selectedText ? 'text-fg-muted' : 'text-fg'">
        <span class="block truncate" :class="small ? 'text-sm' : ''">{{ selectedText || placeholder || '' }}</span>
      </span>
      <span class="ml-3 absolute inset-y-0 right-0 flex items-center pr-2 pointer-events-none">
        <span class="material-symbols">arrow_drop_down</span>
      </span>
    </button>

    <transition name="menu">
      <ul v-show="showMenu" class="absolute z-10 -mt-px w-full bg-primary border border-border shadow-lg max-h-56 rounded-b-md py-1 ring-1 ring-bg ring-opacity-5 overflow-auto focus:outline-none text-sm" role="listbox">
        <template v-for="item in items">
          <li :key="item.value" class="text-fg select-none relative py-2 cursor-pointer hover:bg-black-400" role="option" @click="clickedOption(item.value)">
            <div class="flex items-center">
              <span class="font-normal ml-3 block truncate font-sans text-sm">{{ item.text }}</span>
            </div>
          </li>
        </template>
      </ul>
    </transition>
  </div>
</template>

<script>
export default {
  props: {
    value: [String, Number],
    label: {
      type: String,
      default: ''
    },
    items: {
      type: Array,
      default: () => []
    },
    disabled: Boolean,
    small: Boolean,
    placeholder: String
  },
  data() {
    return {
      showMenu: false
    }
  },
  computed: {
    selected: {
      get() {
        return this.value
      },
      set(val) {
        this.$emit('input', val)
      }
    },
    selectedItem() {
      return this.items.find((i) => i.value === this.selected)
    },
    selectedText() {
      return this.selectedItem ? this.selectedItem.text : ''
    },
    buttonClass() {
      const classes = []
      if (this.small) classes.push('h-9')
      else classes.push('h-10')

      if (this.disabled) classes.push('cursor-not-allowed border-border bg-primary bg-opacity-70 border-opacity-70 text-fg-muted')
      else classes.push('cursor-pointer border-border bg-primary text-fg')

      return classes.join(' ')
    }
  },
  methods: {
    clickShowMenu() {
      if (this.disabled) return
      this.showMenu = !this.showMenu
    },
    clickedOutside() {
      this.showMenu = false
    },
    clickedOption(itemValue) {
      this.selected = itemValue
      this.showMenu = false
    }
  },
  mounted() {}
}
</script>
