<template lang='pug'>
.group-list
  .group-items
    el-card.group-item(v-for="group in groups" :key="group.id" @click.native="selectGroup(group.id)" shadow="hover")
      span {{group.name}}
  el-button(style="width: 100%;", type="default", @click="dialogVisible = true") 新規グループ作成
  el-dialog(title="グループ作成", width="30%", :visible.sync="dialogVisible")
    el-form(:label-position="'top'", :model="groupForm" ref="groupForm")
      el-form-item(label="グループ名")
        el-input(v-model="groupForm.name")
    span.dialog-footer(slot="footer")
      el-button(@click="dialogVisible = false") キャンセル
      el-button(type="primary", @click="createGroup") 作成
</template>
<script>
import { mapState } from 'vuex'
export default {
  data() {
    return {
      dialogVisible: false,
      groupForm: {
        name: ''
      }
    };
  },
  computed: {
    ...mapState({
      groups: state => state.group.groups
    })
  },
  methods: {
    createGroup() {
      this.$store.dispatch('group/createGroup', this.groupForm).then((data) => {
        this.$store.dispatch('group/fetchGroups').then((data) => {
          this.dialogVisible = false
          this.$refs['groupForm'].resetFields()
        })
      });
    },
    selectGroup(groupId) {
      this.$store.dispatch('group/selectGroup', { groupId: groupId })
    }
  },
  mounted() {
    this.$store.dispatch('group/fetchGroups')
  }
}
</script>
<style lang='sass'>
.group-list
  .group-item
    margin: 8px 0
    cursor: pointer
</style>
