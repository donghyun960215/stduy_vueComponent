# Component

## Component 기초
```html
<!-- App.vus -->
<template>
  <MyBtn />
  <MyBtn :color="color" />
  <MyBtn large />
  <MyBtn
    large
    color="red" />
</template>

<script>
import MyBtn from '~/components/MyBtn'

export default {
  components: {
    MyBtn
  },
  data() {
    return {
      color: "#000"
    }
  }
}
</script>
<!-- ////////////////////// -->
<!-- /components/MyBtn.vue -->
<template>
  <div 
    :class="{large: large}"
    :style="{backgroundColor: color}"
    class="btn">
    Apple
  </div>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      default: 'gray'
    },
    large: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style scoped lang="scss">
  .btn {
    display: inline-block;
    margin: 4px;
    padding: 6px 12px;
    border-radius: 4px;
    background-color: gray;
    color: white;
    cursor: pointer;
    &.large {
    font-size: 20px;
    padding: 10px 20px;
  }
  }
  
</style>
```
```plaintext
부모컴포넌트 와 자식컴포넌트의 데이터 통신.
위 와 같이 component를 작성하면 color 부분을 App.vue 에서 내가 원하는 color로 변경이 가능하다.
props 를 통해 color의 값을 지정해주고 상단부분에 :style과 {} 를 사용하여 원하는 속성에 color를 삽입한다.
그렇게 되면 App.vue에서 color를 새로 지정을 할 수 있고 새로 지정한 색이 나오게 된다.
color값 경우에는 data() 사용을 안해도 되고 그냥 색상을 입력해도 무방하다.
props는 여러개도 사용이 가능하다.
```

## Component text 변경
```html
<!-- App.vus -->
<template>
  <MyBtn>Banana</MyBtn>
  <MyBtn :color="color">
    Apple
  </MyBtn>
  <MyBtn large>
    Cherry
  </MyBtn>
  <MyBtn
    large
    color="red">
    Apple
  </MyBtn>
</template>

<script>
import MyBtn from '~/components/MyBtn'

export default {
  components: {
    MyBtn
  },
  data() {
    return {
      color: "#000"
    }
  }
}
</script>
<!-- ////////////////////// -->
<!-- /components/MyBtn.vue -->
<template>
  <div 
    :class="{large: large}"
    :style="{backgroundColor: color}"
    class="btn">
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      default: 'gray'
    },
    large: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style scoped lang="scss">
  .btn {
    display: inline-block;
    margin: 4px;
    padding: 6px 12px;
    border-radius: 4px;
    background-color: gray;
    color: white;
    cursor: pointer;
    &.large {
    font-size: 20px;
    padding: 10px 20px;
  }
  }
</style>
```
```plaintext
App.vue 파일
<MyBtn /> 부분을  <MyBtn><MyBtn /> 으로 변경을 해준다. 그리고 그 사이에 원하는 문자를 입력한다.

MuBtn.vue 파일
문자가 나오고 싶은 부분에 <slot></slot>태그를 추가해준다.
```

## 속성 상속
```html
<!-- App.vus -->
<template>
  <MyBtn
    class="dong"
    style="color: red">
    Banana
  </MyBtn>
</template>

<script>
import MyBtn from '~/components/MyBtn'

export default {
  components: {
    MyBtn
  }
}
</script>
<!-- ////////////////////// -->
<!-- /components/MyBtn.vue -->
<template>
  <div 
    class="btn">
    <slot></slot>
  </div>
  <h1
    :class="$attrs.class"
    :style="$attrs.style">
<!--v-bind = "$attrs" -->
    </h1>
</template>

<script>
export default {
  inheritAttrs: false
}
</script>

<style scoped lang="scss">
  .btn {
    display: inline-block;
    margin: 4px;
    padding: 6px 12px;
    border-radius: 4px;
    background-color: gray;
    color: white;
    cursor: pointer;
  }
</style>
```
```plaintext
App.vue 파일에서 class 와 style 를 추가를 하면 MyBtn의 div의 상속이 된다.
근데 만일 최상위요소가 1개가 아닌 그 이상을 가지게 된다면 상속이 풀리게된다.
즉 div가 한개가 아닌 다른 h1태그나 div의 다른 태그가 있게 된다면 상속은 풀리게된다.

그래서 그걸 방지하고자 MyBtn 의 script 부분에 inheritAttrsf를 추가한다.
그러면 최상위요소가 하나만 있어도 상속을 안입히게 한다.

원하는 요소에 넣고 싶을 때는 $attrs 를 사용해서 넣는다.
만약 따로 넣고 싶은게 없고 그냥 전부다 넣고 싶다면.
v-bind = "$attrs"를 입력해서 사용한다.
```