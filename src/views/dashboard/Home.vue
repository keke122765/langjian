
<template>
  <div>
    <page-view :avatar="avatar" :title="false">
      <div slot="headerContent">
        <div class="title">{{ timeFix }}，{{ user.name }} !</div>
        <div>ID编号：888888</div>
      </div>
    </page-view>
    <div class="page-header-index-wide">
      <!-- 4 cards -->
      <a-row :gutter="24">
        <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
          <chart-card :loading="loading" title="今日收入" total="￥597">
            <a slot="action">查看明细</a>
            <template slot="footer">
              累计收入：
              <span>72234.56 元</span>
            </template>
          </chart-card>
        </a-col>
        <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
          <chart-card :loading="loading" title="在线卡片" total="1024">
            <a slot="action">查看卡片</a>
            <template slot="footer">
              卡片总数：
              <span>4544 张</span>
            </template>
          </chart-card>
        </a-col>
        <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
          <chart-card :loading="loading" title="有效代理" total="1020">
            <a slot="action">查看代理</a>
            <template slot="footer">
              代理总数：
              <span>4544 人</span>
            </template>
          </chart-card>
        </a-col>
        <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
          <chart-card :loading="loading" title="钱包余额" total="112">
            <a slot="action">打开钱包</a>
            <template slot="footer">
              <span>累计充值：529 元</span>
            </template>
          </chart-card>
        </a-col>
      </a-row>

      <!-- chart & top7 -->
      <a-card :loading="loading" :bordered="false" :body-style="{padding: '0'}">
        <div class="salesCard">
          <a-tabs default-active-key="1" size="large" :tab-bar-style="{paddingLeft: '16px'}">
            <a-tab-pane loading="true" tab="营销报表" key="1">
              <a-row>
                <a-col :xl="18" :lg="13" :md="13" :sm="24" :xs="24">
                  <bar title="15日报表"/>
                </a-col>
                <a-col :xl="6" :lg="11" :md="11" :sm="24" :xs="24">
                  <rank-list title="返利排行榜" :list="rankList"/>
                </a-col>
              </a-row>
            </a-tab-pane>
          </a-tabs>
        </div>
      </a-card>
    </div>
  </div>
</template>

<script>
import { timeFix } from '@/utils/util'
import { mapGetters } from 'vuex'

import { PageView } from '@/layouts'
import { Radar, ChartCard, Bar, RankList } from '@/components'

import { getRoleList, getServiceList } from '@/api/manage'

const rankList = []
const nameList = ['刘','彭','袁','郇','陈','闫','阮','欧','孙','刘','张','张','李','谷','杨','姚','黄','董','孙','苏','张','肖','刘','顾','张','许','刘','张','陈','黄','辛','姚','张','范','徐','王','黄','付','吴','张','王','王','李','朱','朱','彭','李','赵','林','司','刘','孔','杨','巴','杨','戴','张','庞','李','胡','贺','王','秦','赵','张','罗','乔','肉','杨','焦','刘','王','宋','李','张'];
console.log(nameList.length)
for (let i = 0; i < 7; i++) {
  rankList.push({
    name: nameList[Math.round(Math.random() * 74)]+(Math.round(Math.random())==1?'**':'*'),
    phone: '188****8888',
    total: 1234.56 - i * 100
  })
}
const DataSet = require('@antv/data-set')

export default {
  name: 'Home',
  components: {
    PageView,
    Radar,
    ChartCard,
    Bar,
    RankList
  },
  data() {
    return {
      rankList,
      timeFix: timeFix(),
      avatar: '',
      user: {},

      projects: [],
      loading: true,
      radarLoading: true,
      activities: [],
      teams: [],

      // data
      axis1Opts: {
        dataKey: 'item',
        line: null,
        tickLine: null,
        grid: {
          lineStyle: {
            lineDash: null
          },
          hideFirstLine: false
        }
      },
      axis2Opts: {
        dataKey: 'score',
        line: null,
        tickLine: null,
        grid: {
          type: 'polygon',
          lineStyle: {
            lineDash: null
          }
        }
      },
      scale: [
        {
          dataKey: 'score',
          min: 0,
          max: 80
        }
      ],
      axisData: [
        { item: '引用', a: 70, b: 30, c: 40 },
        { item: '口碑', a: 60, b: 70, c: 40 },
        { item: '产量', a: 50, b: 60, c: 40 },
        { item: '贡献', a: 40, b: 50, c: 40 },
        { item: '热度', a: 60, b: 70, c: 40 },
        { item: '引用', a: 70, b: 50, c: 40 }
      ],
      radarData: []
    }
  },
  computed: {
    userInfo() {
      return this.$store.getters.userInfo
    }
  },
  created() {
    this.user = this.userInfo
    this.avatar = this.userInfo.avatar

    getRoleList().then(res => {
      console.log('workplace -> call getRoleList()', res)
    })

    getServiceList().then(res => {
      console.log('workplace -> call getServiceList()', res)
    })
  },
  mounted() {
    this.getProjects()
    this.getActivity()
    this.getTeams()
    this.initRadar()
  },
  methods: {
    //...mapGetters(['nickname', 'welcome']),
    getProjects() {
      this.$http.get('/list/search/projects').then(res => {
        this.projects = res.result && res.result.data
        this.loading = false
      })
    },
    getActivity() {
      this.$http.get('/workplace/activity').then(res => {
        this.activities = res.result
      })
    },
    getTeams() {
      this.$http.get('/workplace/teams').then(res => {
        this.teams = res.result
      })
    },
    initRadar() {
      this.radarLoading = true

      this.$http.get('/workplace/radar').then(res => {
        const dv = new DataSet.View().source(res.result)
        dv.transform({
          type: 'fold',
          fields: ['个人', '团队', '部门'],
          key: 'user',
          value: 'score'
        })

        this.radarData = dv.rows
        this.radarLoading = false
      })
    }
  }
}
</script>
<style lang="less" scoped>
.extra-wrapper {
  line-height: 55px;
  padding-right: 24px;

  .extra-item {
    display: inline-block;
    margin-right: 24px;

    a {
      margin-left: 24px;
    }
  }
}

.antd-pro-pages-dashboard-analysis-twoColLayout {
  position: relative;
  display: flex;
  display: block;
  flex-flow: row wrap;

  &.desktop div[class^='ant-col']:last-child {
    position: absolute;
    right: 0;
    height: 100%;
  }
}

.antd-pro-pages-dashboard-analysis-salesCard {
  height: calc(100% - 24px);
  /deep/ .ant-card-head {
    position: relative;
  }
}

.dashboard-analysis-iconGroup {
  i {
    margin-left: 16px;
    color: rgba(0, 0, 0, 0.45);
    cursor: pointer;
    transition: color 0.32s;
    color: black;
  }
}
.analysis-salesTypeRadio {
  position: absolute;
  right: 54px;
  bottom: 12px;
}
</style>

