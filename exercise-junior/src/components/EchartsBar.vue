<template>
  <div>
    <div class="charts_item">
      <div class="echartsBar">
        <div ref="chart" style="width:250px;height:250px"></div>
      </div>
      <div class="info_wrap">
        <img :src="tipIconUrl" alt="">
        <div>
          <p>检测状态</p>
          <p :style="data.residue < 40?'color:gray':data.residue < 75?'color:#80c6fa':'color:red'">{{ data.residue < 40? '未知':data.residue < 75?'正常':'警告' }}</p>
        </div>
      </div>
    </div>
  </div>
</template>
 
<script>
export default {
  name: "EchartsBar",
  components: {},
  props: {
    data: {
      type: Object,
      default: () => {
        return {
          name: "请传值",
          residue: 0,
          total: 100
        };
      }
    }
  },
  data() {
    return {};
  },
  watch: {
    // 当数据发生改变时，重新渲染echarts
    data: {
      handler() {
        this.getEchartsData();
      },
      deep: true,
      immediate: true
    }
  },
  mounted() {
    this.getEchartsData();
  },
  computed: {
    tipIconUrl() {
      return this.data.residue < 40 ? require('../assets/tipIcon/weizhi@3x.png') :
      this.data.residue < 75?require('../assets/tipIcon/zhengchang@3x.png'):require('../assets/tipIcon/baojing@3x.png')
    }
  },
  methods: {
    getEchartsData() {
      const chart = this.$refs.chart;
      if (chart) {
        const myChart = this.$echarts.init(chart);
        const option = {
          title: {
            text: Math.round((this.data.residue / this.data.total) * 100) + "%",
            textStyle: {
              color: "#9371f1",
              fontSize: 28,
              fontWeight: "normal"
            },
            subtext: "异常分数",
            subtextStyle: {
              color: "#48487b",
              fontSize: 20,
              fontWeight: "normal"
            },
            itemGap: 10, // 主副标题距离
            left: "center",
            top: "center"
          },
          angleAxis: {
            max: this.data.total, // 满分
            clockwise: true, // 逆时针
            // 隐藏刻度线
            axisLine: {
              show: false
            },
            axisTick: {
              show: false
            },
            axisLabel: {
              show: false
            },
            splitLine: {
              show: false
            }
          },
          radiusAxis: {
            type: "category",
            // 隐藏刻度线
            axisLine: {
              show: false
            },
            axisTick: {
              show: false
            },
            axisLabel: {
              show: false
            },
            splitLine: {
              show: false
            }
          },
          polar: {
            center: ["50%", "50%"],
            radius: "160%" //图形大小
          },
          series: [
            {
              type: "bar",
              data: [
                {
                  name: "资源剩余量",
                  value: this.data.residue,
                  itemStyle: {
                    normal: {
                      color: 
                        this.$echarts.graphic.LinearGradient(1, 0, 0, 0, [
                              {
                                offset: 0,
                                color: "#9373f1"
                              },
                              {
                                offset: 1,
                                color: "#b19bf2"
                              }
                            ])
                      // color:
                      //   // 比分比低于30%时，显示橘色，否则显示绿色
                      //   Math.round(
                      //     (this.data.residue / this.data.total) * 100
                      //   ) >= 30
                      //     ? this.$echarts.graphic.LinearGradient(1, 0, 0, 0, [
                      //         {
                      //           offset: 0,
                      //           color: "#4FE884"
                      //         },
                      //         {
                      //           offset: 1,
                      //           color: "#e6e9f1"
                      //         }
                      //       ])
                      //     : this.$echarts.graphic.LinearGradient(1, 0, 0, 0, [
                      //         {
                      //           offset: 0,
                      //           color: "#ff4f1e"
                      //         },
                      //         {
                      //           offset: 1,
                      //           color: "#ff8a65"
                      //         }
                      //       ])
                    }
                  }
                }
              ],
              coordinateSystem: "polar",
              roundCap: true,
              barWidth: 30,
              barGap: "-100%", // 两环重叠
              z: 2
            },
            {
              // 灰色环
              type: "bar",
              data: [
                {
                  value: this.data.total,
                  itemStyle: {
                    color: "#e9f1f3",
                    shadowColor: "rgba(0, 0, 0, 0.2)",
                    shadowBlur: 5,
                    shadowOffsetY: 2
                  }
                }
              ],
              coordinateSystem: "polar",
              roundCap: true,
              barWidth: 30,
              barGap: "-100%", // 两环重叠
              z: 1
            }
          ]
        };
        myChart.setOption(option);
        window.addEventListener("resize", function() {
          myChart.resize();
        });
      }
    }
  }
};
</script>
<style scoped lang='less'>
.charts_item {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  justify-items: center;
  align-items: center;
  .info_wrap{
    display: flex;
    justify-content: center;
    width: 250px;
    img{
      width: 50px;
      height: 50px;
      margin-right: 30px;
    }
    p {
      margin: 0;
    }
  }
}
</style>