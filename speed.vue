<template>
  <!-- ################### -->
  <button class="b_gButton" :title="taskName">
    <span class="submit">
      {{ taskName }}
    </span>
    <span class="loading">
      <!-- 进行中图标、文字 -->
      {{ taskName }}&nbsp;&nbsp;({{ speedLoading }}%)
    </span>
    <span class="check"> {{ taskName }}&nbsp;&nbsp;(完成) </span>
  </button>
</template>
<script>
export default {
  props: {
    /* ------静态配置------ */
    // 任务名称
    taskName: {
      type: String,
      default: "未执行任务",
    },
    // 初始显示背景颜色
    initColor: {
      type: String,
      default: "#3498db",
    },
    // 文字显示颜色
    fontColor: {
      type: String,
      default: "#ffffff",
    },
    // 文字显示大小
    fontSize: {
      type: String,
      default: "16",
    },
    // 进度条宽度
    speedWidth: {
      type: String,
      default: "100%",
    },
    // 进度条宽度
    speedHeight: {
      type: String,
      default: "48",
    },
    // 进度条索引(用于多个)
    indexKey: {
      type: Number,
      default: 0,
      required: true,
    },
    /* ------动态配置------ */
    // 进度条加载时背景颜色
    backColor: {
      type: String,
      default: "#27af60",
    },
    // 进度条加载部分颜色
    loadColor: {
      type: String,
      default: "#54d98c",
    },
    /* ------实时配置------ */
    // 进度条完成度
    speedLoading: {
      type: Number,
      default: 0,
    },
  },
  watch: {
    // 进度条宽度
    speedWidth(newData, oldData) {
      let button = document.querySelectorAll(".b_gButton")[this.indexKey];
      if (newData.indexOf("%") != -1) {
        button.style.width = newData;
      } else {
        button.style.width = newData + "px";
      }
    },
    // 进度条高度
    speedHeight(newData, oldData) {
      let button = document.querySelectorAll(".b_gButton")[this.indexKey];
      button.style.height = newData + "px";
    },
    // 进度改变
    speedLoading(newData, oldData) {
      let button = document.querySelectorAll(".b_gButton")[this.indexKey];
      button.classList.add("active");
      // 进度条加载背景颜色
      button.style.setProperty("--backColor", this.backColor);

      //设置css变量
      button.style.setProperty("--beforeWidth", this.speedLoading + "%");
      button.style.setProperty("--beforeLoadColor", this.loadColor);
      //删除变量button.style.removeProperty("--beforeWidth");

      // 延迟过渡效果
      if (newData >= 100) {
        setTimeout(() => {
          button.classList.add("finished");
        }, 400);
      }
    },
  },
  mounted() {
    let button = document.querySelectorAll(".b_gButton")[this.indexKey];
    button.style.setProperty("--backColor", this.initColor);
    button.style.setProperty("--btnWidth", this.speedWidth);
    button.style.setProperty("--fontColor", this.fontColor);
    button.style.setProperty("--fontSize", this.fontSize + "px");
  },
};
</script>
<style scope>
.b_gButton {
  --backColor: #3498db; /* 背景颜色 */
  --btnWidth: 100%; /* 进度条宽度(相对于父元素) */
  --btnHeight: 48px; /* 进度条高度 */
  --beforeWidth: 0%; /* 进度条伪类宽度(进程) */
  --beforeLoadColor: #54d98c; /* 加载进程颜色 */
  --fontColor: #ffffff; /* 文字颜色 */
  --fontSize: 16px; /* 文字大小 */
}
.b_gButton {
  height: var(--btnHeight);
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 1.5rem 3.125rem;
  margin: 20px 0;
  border: none;
  border-radius: 0.3125rem;
  box-shadow: 0 12px 24px 0 rgba(0, 0, 0, 0.2);
  color: var(--fontColor);
  font-size: var(--fontSize);
  width: var(--btnWidth);
  background-color: var(--backColor);
  font-weight: 300;
  text-transform: uppercase;
  overflow: hidden;
}
.b_gButton:before {
  position: absolute;
  content: "";
  bottom: 0;
  left: 0;
  height: 100%;
  width: var(--beforeWidth);
  background-color: var(--beforeLoadColor);
}
.b_gButton span {
  position: absolute;
  line-height: 0;
}
.b_gButton span i {
  transform-origin: center center;
}
.b_gButton span:nth-of-type(1) {
  top: 50%;
  transform: translateY(-50%);
}
.b_gButton span:nth-of-type(2) {
  top: 100%;
  transform: translateY(0%);
  font-size: var(--fontSize);
}
.b_gButton span:nth-of-type(3) {
  display: none;
}
.b_gButton .loading {
  display: none;
}
/* 进度条过程 */
.active:before {
  transition: width 0.3s linear; /* 动画过渡 */
}
.active span:nth-of-type(1) {
  top: -100%;
  transform: translateY(-50%);
}
.active span:nth-of-type(2) {
  top: 50%;
  transform: translateY(-50%);
}
.active span:nth-of-type(2) i {
  animation: loading 800ms linear infinite;
}
.active span:nth-of-type(3) {
  display: none;
}
.active .loading {
  display: block;
}
.finished .submit {
  display: none;
}
.finished .loading {
  display: none;
  width: 100%;
  height: 100%;
}
.finished .check {
  display: block !important;
  font-size: var(--fontSize);
  animation: scale 0.5s linear;
}
.finished .check i {
  transform-origin: center center;
}
/* @keyframes loading {
  100% {
    transform: rotate(-360deg);
  }
} */
@keyframes scale {
  0% {
    transform: scale(10);
  }
  50% {
    transform: scale(0.2);
  }
  70% {
    transform: scale(1.2);
  }
  90% {
    transform: scale(0.7);
  }
  100% {
    transform: scale(1);
  }
}
</style>