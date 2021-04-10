<template>
	<el-page-header @back="goBack" content="详情页面">
	
  <div class="mainbox">
    <el-form ref="elForm" :model="formData" :rules="rules" size="medium" label-width="100px">
      <el-form-item label="案件名称" prop="caseName">
        <el-input v-model="formData.caseName" placeholder="请输入案件名称" clearable :style="{width: '100%'}">
        </el-input>
      </el-form-item>

	    <el-form-item label="案件编号" prop="caseId">
	      <el-input v-model="formData.caseId" placeholder="请输入案件编号" clearable :style="{width: '100%'}">
	      </el-input>
	    </el-form-item>
      <el-form-item label="案件描述" prop="caseDescription">
        <el-input v-model="formData.caseDescription" type="textarea" placeholder="请输入案件描述" show-word-limit
          :autosize="{minRows: 4, maxRows: 4}" :style="{width: '100%'}"></el-input>
      </el-form-item>
      <el-form-item label="案件类型" prop="caseType">
        <el-radio-group v-model="formData.caseType" size="medium">
          <el-radio v-for="(item, index) in caseTypeOptions" :key="index" :label="item.value"
            :disabled="item.disabled">{{item.label}}</el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="可见部门" prop="ids">
        <el-checkbox-group v-model="formData.ids" :min="1"  size="medium">
          <el-checkbox v-for="(item, index) in idsOptions" :key="index" :label="item.value"
            :disabled="item.disabled">{{item.label}}</el-checkbox>
        </el-checkbox-group>
      </el-form-item>
      <el-form-item size="large">
        <el-button type="primary" @click="submitForm">提交</el-button>
        <el-button @click="resetForm">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
  </el-page-header>
</template>
<script>
export default {
  components: {},
  props: [],
  data() {
    return {
      formData: {
		  caseId:'',
        caseName: undefined,
        caseDescription: undefined,
        caseType: '刑事案件',
        ids: [""],
      },
      rules: {
        caseName: [{
          required: true,
          message: '请输入案件名称',
          trigger: 'blur'
        }],
		caseId: [{
		  required: true,
		  message: '请输入案件名称',
		  trigger: 'blur'
		}],
        caseDescription: [{
          required: true,
          message: '请输入案件描述',
          trigger: 'blur'
        }],
        caseType: [{
          required: true,
          message: '案件类型不能为空',
          trigger: 'change'
        }],
        ids: [{
          required: true,
          type: 'array',
          message: '请至少选择一个ids',
          trigger: 'change'
        }],
      },
      caseTypeOptions: [{
        "label": "刑事案件",
        "value": "刑事案件"
      }, {
        "label": "行政案件",
        "value": "行政案件"
      }, {
        "label": "民事案件",
        "value": "民事案件"
      }, {
        "label": "其他",
        "value": "其他"
      }],
      idsOptions: [{
        "label": "公安局",
        "value": 101
      }, {
        "label": "检察院",
        "value": 102
      }, {
        "label": "法院",
        "value": 103
      }, {
        "label": "司法局",
        "value": 104
      }],
    }
  },
  computed: {},
  watch: {},
  created() {},
  mounted() {},
  methods: {
     submitForm() {
      this.$refs['elForm'].validate(async valid => {
        if (!valid) return
		const res= await this.$http.post('/api1/cases/addCase',this.formData)
		console.log(res)
		if(res.data.code == 0){
			  this.$router.push('case')
			  this.$message.success('操作成功！')
		}else
		{
			return this.$message.error('操作失误！')
		}
        // TODO 提交表单
      })
    },
    resetForm() {
      this.$refs['elForm'].resetFields()
    },
  }
}

</script>
<style>
</style>

