<template>
  <div class="bin-json-editor">
    <label>
      <textarea ref="textarea" @blur="handleBlur"/>
    </label>
  </div>
</template>

<script>
  import CodeMirror from 'codemirror'
  import 'codemirror/mode/javascript/javascript'
  import 'codemirror-formatting'
  import 'codemirror/addon/lint/lint'
  import 'codemirror/addon/lint/json-lint'
  import 'codemirror/addon/lint/lint.css'
  import 'codemirror/lib/codemirror.css'
  import 'codemirror/theme/idea.css'
  import 'codemirror/theme/eclipse.css'
  import 'codemirror/theme/rubyblue.css'
  import 'codemirror/theme/duotone-light.css'
  import 'codemirror/theme/monokai.css'
  import 'codemirror/theme/elegant.css'
  import 'codemirror/theme/mdn-like.css'
  import 'codemirror/theme/xq-light.css'
  import 'codemirror/theme/neo.css'
  // eslint-disable-next-line import/no-webpack-loader-syntax
  require('script-loader!jsonlint')

  export default {
    name: 'BCodeEditor',
    props: {
      value: {
        type: String
      },
      showNumber: {
        type: Boolean,
        default: true
      },
      mode: {
        type: String,
        default: 'application/json'
      },
      theme: {
        type: String,
        default: 'idea'
      },
      lint: {
        type: Boolean,
        default: true
      },
      readonly: {
        type: Boolean,
        default: false
      },
      smartIndent: {
        type: Boolean,
        default: true
      },
      lineWrap: {
        type: Boolean,
        default: true
      }
    },
    data() {
      return {
        jsonEditor: false
      }
    },
    watch: {
      value(value) {
        const editorValue = this.jsonEditor.getValue()
        if (value !== editorValue) {
          this.jsonEditor.setValue(value)
          // 触发校验
          this.dispatch('BFormItem', 'on-form-change', value)
        }
      },
      showNumber(val) {
        this.jsonEditor && this.jsonEditor.setOption('lineNumbers', val)
      },
      lint(val) {
        this.jsonEditor && this.jsonEditor.setOption('lint', val)
      },
      readonly(val) {
        this.jsonEditor && this.jsonEditor.setOption('readOnly', val)
      },
      theme(val) {
        this.jsonEditor && this.jsonEditor.setOption('theme', val)
      }
    },
    mounted() {
      this.jsonEditor = CodeMirror.fromTextArea(this.$refs.textarea, {
        lineNumbers: this.showNumber,
        mode: this.mode,
        gutters: ['CodeMirror-lint-markers'],
        theme: this.theme,
        lint: this.lint,
        readOnly: this.readonly,
        smartIndent: this.smartIndent,
        lineWrapping: this.lineWrap, // 代码折叠
        // 快捷键
        extraKeys: {
          'F7': function autoFormat(cm) {
            let totalLines = cm.lineCount()
            cm.autoFormatRange({ line: 0, ch: 0 }, { line: totalLines })
          } // 代码格式化
        }
      })
      this.jsonEditor.setValue(this.value)
      if (this.value && this.value.length > 0) {
        this.formatCode()
      }
      this.jsonEditor.on('change', cm => {
        this.$emit('on-change', cm.getValue())
        this.$emit('input', cm.getValue())
      })
    },
    methods: {
      getValue() {
        return this.jsonEditor.getValue()
      },
      formatCode() {
        if (this.jsonEditor) {
          let totalLines = this.jsonEditor.lineCount()
          this.jsonEditor.autoFormatRange({ line: 0, ch: 0 }, { line: totalLines })
        }
      },
      handleBlur(event) {
        this.$emit('on-blur', event)
        // 触发校验
        this.dispatch('BFormItem', 'on-form-blur', this.value)
      },
      blur() {
        this.$refs.textarea.blur()
        console.log('blur')
      }
    }
  }
</script>
