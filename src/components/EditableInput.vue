<template>
  <div class="container">
    <input
      ref="input"
      v-if="editable"
      @blur="saveChanges"
      @keyup.enter="saveChanges"
      :readonly="!editable"
      class="editable-title"
      :class="{ 'full-width': editable }"
      type="text"
      :value="localValue"
      v-focus
    />
    <div
      v-if="!editable"
      class="input-actions-container"
      @mouseover="showActions = true"
      @mouseleave="showActions = false"
    >
      <p class="title" :class="{ 'full-width': editable }">
        {{ localValue }}
      </p>
      <div class="input-actions" v-if="showActions">
        <button @click.stop="removeItem" v-if="showDeleteAction">
          <svg fill="none" width="16" height="16">
            <use :xlink:href="'#delete-icon'" />
          </svg>
        </button>
        <button @click.stop="hideActionsAndEditInput">
          <svg fill="none" width="16" height="16">
            <use :xlink:href="'#edit-icon'" />
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'EditableInput',
  props: {
    value: {
      type: String,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
    parentId: {
      type: Number,
      default: null,
    },
    showDeleteAction: {
      type: Boolean,
      default: true,
    },
  },
  data: () => ({
    editable: false,
    localValue: '',
    showActions: false,
    deleteIcon: 'delete',
  }),
  watch: {
    value: {
      immediate: true,
      deep: true,
      handler(val) {
        this.$nextTick(() => {
          this.localValue = val
        })
      },
    },
  },
  methods: {
    hideActionsAndEditInput() {
      this.editable = true
      this.showActions = false
    },
    getPayload(event) {
      return {
        value: event.target.value,
        childId: this.index,
        parentId: this.parentId,
      }
    },
    removeItem(event) {
      let payload = { ...this.getPayload(event) }
      this.$emit('remove-item', payload)
    },
    saveChanges(event) {
      this.editable = false
      let payload = { ...this.getPayload(event) }

      this.$emit('save-changes', payload)
    },
  },
  directives: {
    focus: {
      inserted(el) {
        el.focus()
      },
    },
  },
  mounted() {
    this.localValue = this.value
  },
}
</script>

<style lang="css" scoped>
p {
  margin: 0;
  padding: 0;
}
.full-width {
  min-width: 90% !important;
  max-width: 90% !important;
}
.editable-title,
.title {
  color: #66758a;
  border: none;
  padding: 4px;
  border-radius: 4px;
  margin-top: 3px;
  max-width: 250px;
  overflow: hidden;
  overflow-wrap: break-word;
  min-height: 25px;
  max-height: 28px;
}
.editable-title {
  min-width: 78%;
  max-width: 78%;
}
.title {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.editable-title:focus {
  border: none;
  outline: none;
  background-color: #156064;
  color: white;
}
.editable-title:active {
  border: none;
  outline: none;
  background-color: #156064;
  color: white;
}
.editable-title:disabled {
  color: #66758a;
}

.input-actions {
  width: 20%;
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  cursor: pointer;
}
.input-actions-container {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  margin-top: 4px;
  max-width: 100%;
}
</style>
