# identify(生成验证码图片)

>+ This is a digital graphic verification code written in canvas
>+ 这是一个vue的插件，使用canvas来生成图形验证码


## Attributes

| 参数                  |     说明                          |   类型    |   默认值  |
| --------              | :-----:                          | :----:    |  :----:   |
| identifyCode          |   需要展示的验证码                |  string   |   1234    |
| fontSizeMin           |  字体大小，最小值                 |  number   |   16      |
| fontSizeMax           |  字体大小，最大值                 |  number   |   40      |
| backgroundColorMin    |  背景颜色色值最小值，最小为0       |  number   |   180     |
| backgroundColorMax    |  背景颜色色值最大值，最大为255     |  number   |   240     |
| colorMin              |  字体颜色色值最小值，最小为0       |  number   |   50      |
| colorMax              |  字体颜色色值最大值，最大为255     |  number   |   160     |
| lineColorMin          |  干扰线颜色色值最小值，最小为0     |  number   |   40      |
| lineColorMax          |  干扰线颜色色值最大值，最大为255   |  number   |   180     |
| dotColorMin           |  干扰点颜色色值最小值，最小为0     |  number   |   0       |
| dotColorMax           |  干扰点颜色色值最大值，最大为255   |  number   |   255     |
| contentWidth          |  画布宽度                        |  number   |   160     |
| contentHeight         |  画布高度                        |  number   |   40      |

## Demo
```
<template>
  <div id="app" @click="refreshCode">
    <s-identify :identifyCode="identifyCode"></s-identify>
  </div>
</template>

<script>
import SIdentify from './components/identify'
export default {
  name: 'App',
  data () {
    return {
      makeCode: '',
      identifyCode: ''
    }
  },
  mounted () {
    this.makeIdentifyCode({ randomTypeLen: true })
  },
  methods: {
    /**
     * 随机生成一个0~9之间的数
     */
    randomNum () {
      return Math.floor(Math.random() * 10)
    },

    /**
     * 随机生成一个字母
     * @param {boolean} isUupper 生成大写字母
     */
    randomAlphabet ({ isUupper = false } = {}) {
      // a的Unicode值为97,z的Unicode值为123
      const alphabet = String.fromCharCode(Math.floor(Math.random() * 25) + 97)
      if (!isUupper) {
        return alphabet
      } else {
        return alphabet.toUpperCase()
      }
    },

    /**
     * 生成图片验证码
     * @param {number} length 图片验证码位数
     * @param {boolean} typeMix 数字和字母混合
     * @param {string} pureNumber 纯数字('number')或者字母('alphabet')
     * @param {boolean} randomTypeLen 随机生成类型个数组合
     * @param {boolean} capsLookMix 字母大小写混合
     * @param {number} numLength 混合类型下的数字个数
     * @param {number} uupperLength 大写字母的个数
     */
    makeIdentifyCode ({ length = 4, typeMix = true, pureNumber = 'alphabet', randomTypeLen = false, capsLookMix = false, numLength = 2, uupperLength = 1 } = {}) {
      this.makeCode = ''
      // 数字和字母混合
      if (typeMix) {
        // 随机生成类型个数组合
        if (randomTypeLen) {
          // 字母大小写混合
          if (capsLookMix) {
            const numLength = Math.floor(Math.random() * length) + 1
            const uupperLength = numLength === length ? 0 : Math.floor(Math.random() * (length - numLength)) + 1
            for (let i = 0; i < numLength; i++) {
              this.makeCode += this.randomNum()
            }
            for (let i = 0; i < uupperLength; i++) {
              this.makeCode += this.randomAlphabet({ isUupper: true })
            }
            if (numLength + uupperLength < length) {
              for (let i = 0; i < length - numLength - uupperLength; i++) {
                this.makeCode += this.randomAlphabet()
              }
            }
          } else {
            const numLength = Math.floor(Math.random() * length) + 1
            for (let i = 0; i < numLength; i++) {
              this.makeCode += this.randomNum()
            }
            if (numLength < length) {
              for (let i = 0; i < length - numLength; i++) {
                this.makeCode += this.randomAlphabet()
              }
            }
          }
        } else {
          // 字母大小写混合
          if (capsLookMix) {
            const tempNumLength = numLength < 0 ? 2 : numLength > length ? 2 : numLength
            const tempUupperLength = uupperLength < 0 ? 1 : uupperLength > length - tempNumLength ? 1 : uupperLength
            for (let i = 0; i < tempNumLength; i++) {
              this.makeCode += this.randomNum()
            }
            for (let i = 0; i < tempUupperLength; i++) {
              this.makeCode += this.randomAlphabet({ isUupper: true })
            }
            if (tempNumLength + tempUupperLength < length) {
              for (let i = 0; i < length - tempNumLength - tempUupperLength; i++) {
                this.makeCode += this.randomAlphabet()
              }
            }
          } else {
            const tempNumLength = numLength < 0 ? 2 : numLength > length ? 2 : numLength
            for (let i = 0; i < tempNumLength; i++) {
              this.makeCode += this.randomNum()
            }
            if (tempNumLength < length) {
              for (let i = 0; i < length - tempNumLength; i++) {
                this.makeCode += this.randomAlphabet()
              }
            }
          }
        }
      } else {
        // 纯数字('number')
        if (pureNumber === 'number') {
          for (let i = 0; i < length; i++) {
            this.makeCode += this.randomNum()
          }
        }
        // 纯字母('alphabet')
        if (pureNumber === 'alphabet') {
          // 字母大小写混合
          if (capsLookMix) {
            const tempUupperLength = uupperLength < 0 ? 1 : uupperLength > length ? 1 : uupperLength
            for (let i = 0; i < tempUupperLength; i++) {
              this.makeCode += this.randomAlphabet({ isUupper: true })
            }
            if (tempUupperLength < length) {
              for (let i = 0; i < length - tempUupperLength; i++) {
                this.makeCode += this.randomAlphabet()
              }
            }
          } else {
            for (let i = 0; i < length; i++) {
              this.makeCode += this.randomAlphabet()
            }
          }
        }
      }
      this.shuffle(this.makeCode)
    },

    /**
     * 图片验证码随机排序
     * @param {string} str 图片验证码
     */
    shuffle (str) {
      this.identifyCode = [...str].sort(() => Math.random() - 0.5).join('')
      console.log(this.identifyCode)
    },

    /**
     * 更换图片验证码
     */
    refreshCode () {
      this.makeIdentifyCode({ randomTypeLen: true })
    }
  },
  components: {
    's-identify': SIdentify
  }
}
</script>

```
## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

npm test
```