使用方式：

1·直接下载vue文件

2·将该组件放到指定位置后，在需要用到的地方直接引入，例：

    import Speed from "../../components/ins/speed.vue";
3·配置属性，例：

    <Speed
      v-for="(item, index) in tabData"
      :indexKey="index"
      :speedLoading="item.step"
      taskName="红外测温"
      @click="speedStep(item)"
    ></Speed>
