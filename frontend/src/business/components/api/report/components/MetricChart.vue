<template>
  <div class="metric-container">
    <el-row type="flex" align="middle">
      <div style="width: 50%">
        <el-row type="flex" justify="center" align="middle">
          <chart id="chart" ref="chart" :options="options" :autoresize="true"></chart>
          <el-row type="flex" justify="center" align="middle">
            <i class="circle success"/>
            <div class="metric-box">
              <div class="value">{{content.success}}</div>
              <div class="name">{{$t('api_report.success')}}</div>
            </div>
            <div style="width: 40px"></div>
            <i class="circle fail"/>
            <div class="metric-box">
              <div class="value">{{content.error}}</div>
              <div class="name">{{$t('api_report.fail')}}</div>
            </div>
          </el-row>
        </el-row>
      </div>
      <div class="split"></div>
      <div style="width: 50%">
        <el-row type="flex" justify="space-around" align="middle">
          <div class="metric-icon-box">
            <i class="el-icon-warning-outline fail"></i>
            <div class="value">{{fail}}</div>
            <div class="name">{{$t('api_report.fail')}}</div>
          </div>
          <div class="metric-icon-box">
            <i class="el-icon-document-checked assertions"></i>
            <div class="value">{{assertions}}</div>
            <div class="name">{{$t('api_report.assertions_pass')}}</div>
          </div>
          <div class="metric-icon-box">
            <i class="el-icon-document-copy total"></i>
            <div class="value">{{this.content.total}}</div>
            <div class="name">{{$t('api_report.request')}}</div>
          </div>
        </el-row>
      </div>
    </el-row>
  </div>
</template>

<script>
  export default {
    name: "MsMetricChart",

    props: {
      content: Object
    },

    computed: {
      options() {
        return {
          color: ['#67C23A', '#F56C6C'],
          tooltip: {
            trigger: 'item',
            formatter: '{b}: {c} ({d}%)'
          },
          title: [{
            text: '{value|' + this.content.total + '}\n{name|' + this.$t('api_report.request') + '}',
            top: 'center',
            left: 'center',
            textStyle: {
              rich: {
                align: 'center',
                value: {
                  fontSize: 32,
                  fontWeight: 'bold',
                  padding: [10, 0]
                },
                name: {
                  fontSize: 14,
                  fontWeight: 'normal',
                  color: '#7F7F7F',
                }
              }
            }
          }],
          series: [
            {
              type: 'pie',
              radius: ['80%', '90%'],
              avoidLabelOverlap: false,
              hoverAnimation: false,
              itemStyle: {
                normal: {
                  borderColor: "#FFF",
                  shadowColor: '#E1E1E1',
                  shadowBlur: 10
                }
              },
              labelLine: {
                show: false
              },
              data: [
                {value: this.content.success, name: this.$t('api_report.success')},
                {value: this.content.error, name: this.$t('api_report.fail')},
              ]
            }
          ]
        };
      },

      fail() {
        return (this.content.error / this.content.total).toFixed(0) + "%";
      },
      assertions() {
        return this.content.passAssertions + " / " + this.content.totalAssertions;
      }
    },
  }
</script>

<style scoped>
  .metric-container {
    padding: 20px;
  }

  .metric-container #chart {
    width: 140px;
    height: 140px;
    margin-right: 40px;
  }

  .metric-container .split {
    margin: 20px;
    height: 100px;
    border-left: 1px solid #D8DBE1;
  }

  .metric-container .circle {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    box-shadow: 0 0 20px 1px rgba(200, 216, 226, .42);
    display: inline-block;
    margin-right: 10px;
    vertical-align: middle;
  }

  .metric-container .circle.success {
    background-color: #67C23A;
  }

  .metric-container .circle.fail {
    background-color: #F56C6C;
  }

  .metric-box {
    display: inline-block;
    text-align: center;
  }

  .metric-box .value {
    font-size: 32px;
    font-weight: 600;
    letter-spacing: -.5px;
  }

  .metric-box .name {
    font-size: 16px;
    letter-spacing: -.2px;
    color: #404040;
  }

  .metric-icon-box {
    text-align: center;
    margin: 0 10px;
  }

  .metric-icon-box .value {
    font-size: 20px;
    font-weight: 600;
    letter-spacing: -.4px;
    line-height: 28px;
    vertical-align: middle;
  }

  .metric-icon-box .name {
    font-size: 13px;
    letter-spacing: 1px;
    color: #BFBFBF;
    line-height: 18px;
  }

  .metric-icon-box .fail {
    color: #F56C6C;
    font-size: 40px;
  }

  .metric-icon-box .assertions {
    font-size: 40px;
  }

  .metric-icon-box .total {
    font-size: 40px;
  }

</style>