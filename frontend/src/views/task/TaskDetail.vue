<template>
  <div class="app-container">
    <!--tabs-->
    <el-tabs v-model="activeTabName" @tab-click="onTabClick" type="card">
      <el-tab-pane :label="$t('Overview')" name="overview">
        <task-overview/>
      </el-tab-pane>
      <el-tab-pane :label="$t('Log')" name="log">
        <div class="log-view">
          <pre>
            {{taskLog}}
          </pre>
        </div>
      </el-tab-pane>
      <el-tab-pane :label="$t('Results')" name="results">
        <general-table-view :data="taskResultsData"
                            :columns="taskResultsColumns"
                            :page-num="resultsPageNum"
                            :page-size="resultsPageSize"
                            :total="taskResultsTotalCount"/>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import {
  mapState
} from 'vuex'
import TaskOverview from '../../components/Overview/TaskOverview'
import GeneralTableView from '../../components/TableView/GeneralTableView'

export default {
  name: 'TaskDetail',
  components: {
    GeneralTableView,
    TaskOverview
  },
  data () {
    return {
      activeTabName: 'overview'
    }
  },
  computed: {
    ...mapState('task', [
      'taskLog',
      'taskResultsData',
      'taskResultsColumns',
      'taskResultsTotalCount'
    ]),
    ...mapState('file', [
      'currentPath'
    ]),
    ...mapState('deploy', [
      'deployList'
    ]),
    resultsPageNum: {
      get () {
        return this.$store.state.task.resultsPageNum
      },
      set (value) {
        this.$store.commit('task/SET_RESULTS_PAGE_NUM', value)
      }
    },
    resultsPageSize: {
      get () {
        return this.$store.state.task.resultsPageSize
      },
      set (value) {
        this.$store.commit('task/SET_RESULTS_PAGE_SIZE', value)
      }
    }
  },
  methods: {
    onTabClick () {
    },
    onSpiderChange (id) {
      this.$router.push(`/spiders/${id}`)
    }
  },
  created () {
    this.$store.dispatch('task/getTaskData', this.$route.params.id)
    this.$store.dispatch('task/getTaskLog', this.$route.params.id)
    this.$store.dispatch('task/getTaskResults', this.$route.params.id)
  }
}
</script>

<style scoped>

  .selector {
    display: flex;
    align-items: center;
    position: absolute;
    right: 20px;
    /*float: right;*/
    z-index: 999;
    margin-top: -7px;
  }

  .selector .el-select {
    padding-left: 10px;
  }
</style>
