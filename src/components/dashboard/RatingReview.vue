<template>
  <div class="rating-review-wrap">

    <div v-show="!isChart">
      <div class="table-nav clearfix">
        <h3><i class="color-line color4"></i>Review & Rating Performance Overview</h3>
        <div class="tool-box clearfix">
          <div class="selection-box">
            <selection :selections="selectListOne" class="select-colorOrange" @selectShow="selectShowOneHandle"
                       ref="selectOptionOne"></selection>
          </div>
          <div class="icon-box">
            <span title="Show Chartbar" class="none" @click="chartShowButton(0)">
              <svg-icon sign="icon-chartbar" class="chart-icon"></svg-icon>
            </span>
            <span class="qrcode" @mouseenter="qrcodeShow" @mouseleave="qrcodeHide">
              <svg-icon sign="icon-erweima" class="erweima-icon"></svg-icon>
              <div class="qrcode-warp" v-show="isQrShow">
                <div class="qrcodeCanvas"></div>
                <span>Please scan the QR code</span>
              </div>
            </span>
            <span @click="copyURL" title="Click to copy this page link">
            <svg-icon sign="icon-link" class="link-icon"></svg-icon>
          </span>
          </div>
        </div>
      </div>
      <div class="table-name">Year-to-date</div>
      <div class="table-box">
        <table id="yearTable" class="data-table" style="width:100%">
          <thead>
          <tr>
            <th>
              <div>Category</div>
            </th>
            <th>
              <div>Rating</div>
            </th>
            <th>
              <div>Over 4.3</div>
            </th>
            <th>
              <div>Below 4.5</div>
            </th>
            <th>
              <div>Over 4.8</div>
            </th>
            <th>
              <div>Sellout volume</div>
            </th>
            <th>
              <div>Total review</div>
            </th>
            <th>
              <div>% of review</div>
            </th>
            <th>
              <div>Positive</div>
            </th>
            <th>
              <div>Neutral</div>
            </th>
            <th>
              <div>Negative</div>
            </th>
            <th>
              <div>% of negative</div>
            </th>
            <th>
              <div>Timely Response</div>
            </th>
            <th>
              <div>Timely Response %</div>
            </th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="(item, index) in yearTableData" :class="{odd: index%2 == 0, even: index%2 != 0}"
              v-if="yearTableData.length >= 0">

            <td>
              <div class="floatL">{{item.category}}</div>
            </td>
            <td v-if="channelId ==0">
              <div>-</div>
            </td>
            <td v-else="channelId != 0">
              <div>{{item.avgRating | round}}</div>
            </td>
            <td v-if="channelId ==0">
              <div>-</div>
            </td>
            <td v-else="channelId != 0">
              <div>{{item.smallVolume | percentile}}</div>
            </td>
            <td v-if="channelId ==0">
              <div>-</div>
            </td>
            <td v-else="channelId != 0">
              <div>{{item.midVolume | percentile}}</div>
            </td>
            <td v-if="channelId ==0">
              <div>-</div>
            </td>
            <td v-else="channelId != 0">
              <div>{{item.bigVolume | percentile}}</div>
            </td>
            <td>
              <div>{{item.selloutVolume | formatThousands}}</div>
            </td>
            <td>
              <div>{{item.totalReview | formatThousands}}</div>
            </td>
            <td>
              <div>{{item.percentReview | percentile}}</div>
            </td>
            <td>
              <div>{{item.positive | formatThousands}}</div>
            </td>
            <td>
              <div>{{item.neutral | formatThousands}}</div>
            </td>
            <td>
              <div>{{item.negative | formatThousands}}</div>
            </td>
            <td>
              <div>{{item.percentNegative | percentile}}</div>
            </td>
            <td v-if="channelId ==0">
              <div>-</div>
            </td>
            <td v-else="channelId != 0">
              <div>{{item.timelyResponse | strikethroughOne}}</div>
            </td>
            <td v-if="channelId ==0">
              <div>-</div>
            </td>
            <td v-else="channelId != 0">
              <div>{{item.timelyResponseRate | strikethrough}}</div>
            </td>
          </tr>
          <tr v-else>
            <td class="noData" :colspan="14">
              <div>Temporarily no data</div>
            </td>
          </tr>
          </tbody>
        </table>
      </div>
      <!---------------->
      <div class="table-name">By month</div>
      <div>
        <table id="monthTable" class="data-table" style="width:100%">
          <thead>
          <tr>
            <th>
              <div>Category</div>
            </th>
            <th>
              <div>Rating</div>
            </th>
            <th>
              <div>Over 4.3</div>
            </th>
            <th>
              <div>Below 4.5</div>
            </th>
            <th>
              <div>Over 4.8</div>
            </th>
            <th>
              <div>Sellout volume</div>
            </th>
            <th>
              <div>Total review</div>
            </th>
            <th>
              <div>% of review</div>
            </th>
            <th>
              <div>Positive</div>
            </th>
            <th>
              <div>Neutral</div>
            </th>
            <th>
              <div>Negative</div>
            </th>
            <th>
              <div>% of negative</div>
            </th>
            <th>
              <div>Timely Response</div>
            </th>
            <th>
              <div>Timely Response %</div>
            </th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="(item, index) in monthTableData" :class="{odd: index%2 == 0, even: index%2 != 0}"
              v-if="monthTableData.length >= 0">
            <td>
              <div class="floatL">{{item.category}}</div>
            </td>
            <td>
              <div>{{item.avgRating | round}}</div>
            </td>
            <td>
              <div>{{item.smallVolume | percentile}}</div>
            </td>
            <td>
              <div>{{item.midVolume | percentile}}</div>
            </td>
            <td>
              <div>{{item.bigVolume | percentile}}</div>
            </td>
            <td>
              <div>{{item.selloutVolume | formatThousands}}</div>
            </td>
            <td>
              <div>{{item.totalReview | formatThousands}}</div>
            </td>
            <td>
              <div>{{item.percentReview | percentile}}</div>
            </td>
            <td>
              <div>{{item.positive | formatThousands}}</div>
            </td>
            <td>
              <div>{{item.neutral | formatThousands}}</div>
            </td>
            <td>
              <div>{{item.negative | formatThousands}}</div>
            </td>
            <td>
              <div>{{item.percentNegative | percentile}}</div>
            </td>
            <td v-if="channelId ==0">
              <div>-</div>
            </td>
            <td v-else="channelId != 0">
              <div>{{item.timelyResponse | strikethroughOne}}</div>
            </td>
            <td v-if="channelId ==0">
              <div>-</div>
            </td>
            <td v-else="channelId != 0">
              <div>{{item.timelyResponseRate | strikethrough}}</div>
            </td>
          </tr>
          <tr v-else>
            <td class="noData" :colspan="14">
              <div>Temporarily no data</div>
            </td>
          </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!---------------------------------------------------------------------------------------------------------------->

    <div class="chart-wrap" :class="{active:isChart}">
      <div class="table-nav clearfix">
        <h3><i class="color-line color4"></i>Review & Rating Performance Overview</h3>
        <div class="tool-box clearfix">
          <div class="selection-box">
            <selection :selections="selectListOne" class="select-colorOrange" @selectShow="selectShowOneHandle" ref="selectOptionTwo"></selection>
          </div>
          <div class="chart-icon-box" @click="chartShowButton(1)">
            <svg-icon sign="icon-arrow-left-c" class="arrow-icon"></svg-icon><span class="back-botton">Back</span>
          </div>
        </div>
      </div>
      <div class="chart-cont-wrap RR">
        <div class="chart-cont" ref="myEchart"></div>
      </div>
    </div>

  </div>
</template>

<script type="text/ecmascript-6">
  import xhrUrls from '../../assets/config/xhrUrls';
  import {getQueryString, getHashString} from '../../assets/config/urlQuery';
  import {get, post} from '../../assets/config/http';

  const RV_SEARCH = xhrUrls.RV_SEARCH;
  const RV_CHANNEL = xhrUrls.RV_CHANNEL;
  const RV_SEARCHCHART = xhrUrls.RV_SEARCHCHART;
  export default {
    name: "rating-review",
    data() {
      return {
        yearTableData: [],
        monthTableData: [],
        selectListOne: ['All Channels'],
        channel: null,
        channelId: 0,
        searchDataYear: {
          isYTD: true,
          month: '',
          channel: ''
        },
        searchDataMonth: {
          isYTD: false,
          month: "",
          channel: ''
        },
        chartData: {
          month: "",
          channel: ''
        },
        url: '',
        isQrShow: false,
        isChart:false,
        lineOption:{
          silent:true,
          data : [
            {
              yAxis: '4.3',
              lineStyle:{
                normal: {
                  type: 'dashed',
                  color:'#00B0F0'
                }
              },
              label:{
                formatter: "4.3%"
              }
            },
            {
              yAxis: 4.5,
              lineStyle:{
                normal: {
                  type: 'dashed',
                  color:'#FF7A35'
                }
              },
              label:{
                formatter: "4.5%"
              }
            },
            {
              yAxis: 4.8,
              lineStyle:{
                normal: {
                  type: 'dashed',
                  color:'#68A490'
                }
              },
              label:{
                formatter: "4.8%"
              }
            }
          ]
        },
        chartOption:{
          tooltip: {
            trigger: "axis",
            confine: true,
            axisPointer: { // 坐标轴指示器，坐标轴触发有效
              type: "shadow" // 默认为直线，可选为："line" | "shadow"
            }
          },
          barWidth: 20,
          color:['#A9D18E'],
          grid: {
            left: "1%",
            right: "4%",
            bottom: "1%",
            top: "1%",
            containLabel: true
          },
          legend: {
            data: '',
            show:false,
            textStyle:{
              color:'#7f7f7f'
            }
          },
          xAxis: [{
            type: 'category',
            data: '',
            axisPointer: {
              type: 'shadow'
            },
            axisLine: {
              lineStyle:{
                type: 'solid',
                color:'#9f9f9f'
              },
            },
            axisLabel: {
              show: true,
              textStyle: {
                color: '#7f7f7f'
              }
            },
          }],
          yAxis: [
            {
              type: 'value',
              name: '',
              min: 0,
              axisLabel: {
                formatter: "{value}%"
              },
              axisLine: {
                lineStyle:{
                  type: 'solid',
                  color:'#9f9f9f'
                },
              },
              axisLabel: {
                show: true,
                textStyle: {
                  color: '#7f7f7f'
                }
              },
              splitLine:{
                show: true,
                lineStyle:{
                  type: 'solid',
                  color:'#9f9f9f'
                },
              }
            }
          ],
          series: ''
        }
      }
    },
    computed: {
      getYearMonth() {
        return this.$store.getters.getYearMonth
      },
      EchartsInit() {
        return this.$echarts.init(this.$refs.myEchart)
      }
    },
    mounted() {

      this.$Hub.$on('ReviewRatingUploadData', ()=>{
        this.getYeartableData()
        this.getMonthtableData()
      })

      if (window.location.hash.indexOf("?") != -1) {
        this.locationHash = true
      } else {
        this.locationHash = false
      }

      if (this.locationHash) {
        this.getUrl()
      } else {
        this.getChannelList()
        this.getYeartableData()
        this.getMonthtableData()
        this.getChartData()
      }
      window.onresize = this.EchartsInit.resize
    },
    updated() {
      this.locationHash = false
    },
    methods: {

      getYeartableData() {
        this.searchDataYear.channel = this.channel
        this.searchDataYear.month = this.getYearMonth
        post(RV_SEARCH, this.searchDataYear).then(res => {
          let data = res.data
          if (data.code == 200) {
            this.yearTableData = data.data.data
          } else {
            console.log(data.errMsg)
          }
        }).catch(err => console.log(err))
      },

      getMonthtableData() {
        this.searchDataMonth.channel = this.channel
        this.searchDataMonth.month = this.getYearMonth
        post(RV_SEARCH, this.searchDataMonth).then(res => {
          let data = res.data
          if (data.code == 200) {
            this.monthTableData = data.data.data
          } else {
            console.log(data.errMsg)
          }
        }).catch(err => console.log(err))
      },

      getChartData(){
        this.chartData.channel = this.channel
        this.chartData.month = this.getYearMonth
        post(RV_SEARCHCHART,this.chartData).then(res => {
          let data = res.data.data
          data.series[0].markLine = this.lineOption
          this.chartOption.legend.data = data.legend
          this.chartOption.xAxis[0].data = data.xAxis
          this.chartOption.series = data.series
          this.Echarts()
        }).catch(err => console.log(err))
      },

      getChannelList() {
        get(RV_CHANNEL).then(res => {
          let data = res.data.data
          data.forEach(val => {
            this.selectListOne.push(val)
          })
        })
      },

      copyURL() {
        this.url = `${window.location}?yearMonth=${this.getYearMonth}&channel=${this.channel}&channelId=${this.channelId}`;
        const input = document.createElement('input')
        document.body.appendChild(input)
        input.setAttribute('value', this.url)
        input.select()
        if (document.execCommand('copy')) {
          document.execCommand('copy')
          this.layerMsg("Page URL has been copied successfully!")
        }
        document.body.removeChild(input)
      },

      layerMsg(err) {
        layer.msg(err, {
          time: 2000,
          skin: 'fontColor'
        })
      },

      qrcodeShow() {
        let baseUrl, qrcodeUrl;
        if(QRCODE_URL){
          qrcodeUrl = `http://www.philipsreport.cn`;
        }else{
          qrcodeUrl = `${window.location}`;
        }
        baseUrl = `${qrcodeUrl}?yearMonth=${this.getYearMonth}&channel=${this.channel}&channelId=${this.channelId}`;
        $('.qrcodeCanvas').html('')
        $('.qrcodeCanvas').qrcode({width: 150,height: 150,text: baseUrl});
        this.isQrShow = true
      },

      qrcodeHide() {
        this.isQrShow = false
      },

      tips(i, id) {
        let tipsVal = ''
        switch (i) {
          case 0:
            tipsVal = 'The process of clicking through an online advertisement to the advertiser’s destination.'
            this.tipsContent(tipsVal, id)
            break
          case 3:
            tipsVal = 'The bounce rate is the percentage of visits with only 1 page view.'
            this.tipsContent(tipsVal, id)
            break
          case 4:
            tipsVal = 'Buy lead conversion shows the value of the conversion points used on the Philips digital platform. For B2C, the conversion point refers to the buy button clicks. For B2B, conversion points are represented by leads generated for completed HS lead form.'
            this.tipsContent(tipsVal, id)
            break
          case 5:
            tipsVal = 'User goal complete rate. It is about consumer survey bounced upon Philips website when viewing it.'
            this.tipsContent(tipsVal, id)
            break
          case 6:
            tipsVal = 'MKT qualified lead.'
            this.tipsContent(tipsVal, id)
            break
          case 7:
            tipsVal = 'Sales qualified leads.'
            this.tipsContent(tipsVal, id)
            break
          default:
            break
        }
      },

      tipsContent(tipsVal, id) {
        layer.tips(tipsVal, '#' + id, {
          tips: [2, '#FFEDB2'],
          skin: 'fontColorBg',
        });
      },

      selectShowOneHandle(val) {

        if (!this.locationHash) {

          val.val == 'All Channels' ? this.channel = null : this.channel = val.val
          this.channelId = val.id

          this.$refs.selectOptionOne.nowIndex = val.id

          this.$refs.selectOptionTwo.nowIndex = val.id

        }

      },

      getUrl() {

        let channel = getHashString('channel')

        let channelId = getHashString('channelId')

        this.getChannelList()

        channel == 'null' ? this.channel = null : this.channel = channel

        this.$refs.selectOptionOne.nowIndex = channelId

        this.$refs.selectOptionTwo.nowIndex = channelId

        this.getYeartableData()

        this.getMonthtableData()

        this.getChartData()

      },

      Echarts() {
        this.EchartsInit.setOption(this.chartOption, {
          notMerge: true
        });
      },

      chartShowButton(type){
        if(type==0){
          this.isChart=true
        }else if(type==1){
          this.isChart=false
        }
      }

    },
    filters: {
      formatThousands: (params) => {
        if (!params) return 0
        let str = Math.round(params).toFixed(0)
        return (str + '').replace(/\d{1,3}(?=(\d{3})+(\.\d*)?$)/g, '$&,');
      },
      percentile: (params) => {
        if (!params) return  '0.00%'
        return (Number(params) * 100).toFixed(2) + '%'
      },
      round: (params) => {
        if (!params) return 0
        return params.toFixed(2)
      },
      strikethroughOne: (params) => {
        if (!params) return '-'
        return params
      },
      strikethrough: (params) => {
        if (!params) return '-'
        return (Number(params) * 100).toFixed(2) + '%'
      }
    },
    watch: {
      channel() {
        if (!this.locationHash) {
          this.getYeartableData()
          this.getMonthtableData()
          this.getChartData()
        }
      },
      getYearMonth() {
        this.getYeartableData()
        this.getMonthtableData()
        this.getChartData()
      }
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "../appmain/appmain.styl"
  .data-table
    border-collapse collapse
    border none
    table-layout fixed
    thead > tr
      background #92D050
      th
        padding 10px 0
        border 1px solid #ddd
        cursor pointer
        color #fff
        font-weight normal
        font-style italic
        white-space pre-wrap
        word-wrap break-word
        &:nth-child(1)
          width 70px
        &:nth-child(2)
          width 50px
  tbody > tr
    &.odd
      background-color #F2F2F2
    &.even
      background-color #FFFFFF
    td
      padding 10px 0
      border 1px solid #ddd
      text-align center
      white-space pre-wrap
      word-wrap break-word
      &.noData
        font-size 14px
        padding 10px 0
        text-align center
    .hidden
      display none

  @media screen and (max-device-width: 1235px) and (-webkit-min-device-pixel-ratio: 2) , (min-device-pixel-ratio: 2) , (-webkit-min-device-pixel-ratio: 2.75) , (min-device-pixel-ratio: 2.75) , (-webkit-min-device-pixel-ratio: 3) , (min-device-pixel-ratio: 3)
    .data-table
      tr
        td
        th
          &:nth-child(6)
            width 75px!important /*130*/
          &:nth-child(13)
            width 60px!important /*130*/
          &:nth-child(14)
            width 60px!important /*150*/
          padding 0 !important
          div
            transform scale(.7)

  @media screen and (orientation: landscape),(-webkit-orientation: landscape)
    .data-table
      tr
        td
        th
          &:nth-child(6)
            width auto!important
          &:nth-child(13)
            width auto!important
          &:nth-child(14)
            width 135px!important
</style>
