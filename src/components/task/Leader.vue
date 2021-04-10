<template>
	<div class="mainbox">
	<el-tabs v-model="leaderName" @tab-click="leaderClick">
		<el-tab-pane label="待审核" name="leader1">
			<div class="text-item">案件审核</div>
			<el-table :data="nocheck" stripe style="width: 100%">
		
				<el-table-column sortable prop="createDate" label="案发时间"></el-table-column>
					<el-table-column  prop="caseName" label="案件名称"></el-table-column>
				<el-table-column prop="caseDescription" label="案件概述"></el-table-column>
			<el-table-column prop="staffId" label="负责民警">
				<template slot-scope="{row}">
					<span v-if="row.staffId== '1001'">张志和</span>
					<span v-if="row.staffId== '1002'">谢燕</span>
					<span v-if="row.staffId== '1003'">王爱国</span>
					<span v-if="row.staffId== '1004'">李建国</span>
				</template>
			</el-table-column>
				<el-table-column prop="caseTypeId" label="案件类型"></el-table-column>
				<el-table-column label="操作" width="150">
					<!-- eslint-disable -->
					<template slot-scope="scope">
						
							<el-button type="primary"  size="mini" @click="lookevidence">查看卷宗</el-button>
						
					</template>
				</el-table-column>
				<el-table-column label="审核" width="150">
			
					<!-- 审核按钮 -->
					<template slot-scope="{row}">
					<el-button type="success"  size="mini" @click="pass(row.caseId)">通过</el-button>
					</template>
			
				</el-table-column>
			</el-table>
		</el-tab-pane>
		
	</el-tabs>
	
	<!-- 	<div class="block">
			<el-row>
				<el-col span="24">
					<el-timeline>
						<el-timeline-item timestamp="2018/4/12" placement="top">
							<el-card>
								<h4>更新 Github 模板</h4>
								<p>王小虎 提交于 2018/4/12 20:46</p>
							</el-card>
						</el-timeline-item>
						<el-timeline-item timestamp="2018/4/3" placement="top">
							<el-card>
								<h4>更新 Github 模板</h4>
								<p>王小虎 提交于 2018/4/3 20:46</p>
							</el-card>
						</el-timeline-item>
						<el-timeline-item timestamp="2018/4/2" placement="top">
							<el-card>
								<h4>更新 Github 模板</h4>
								<p>王小虎 提交于 2018/4/2 20:46</p>
							</el-card>
						</el-timeline-item>
					</el-timeline>
				</el-col>
			</el-row>
		</div> -->
	</div>
</template>

<script>
	export default {
		data() {
			return {
				leaderName: 'leader1',
				nocheck: [],
			}
			
		},
		created() {
			this.noCheck()
		},
		methods: {
			leaderClick(tab, event) {
				console.log(tab, event);
			},
		async	pass(id) {
				console.log(id)
				const res = await this.$http.post('/api1/cases/updateCase',{caseId:id})
				console.log(res)
				if (res.data.code == 0){
					this.$message.success('审核通过！')
					this.noCheck()
				} 
			},
			async noCheck() {
				const {
					data: res
				} = await this.$http.post('/api1/cases/getTypeCase',{status:1})
				if (res.data.code !== 0) return this.$message.error('获取未审核数据失败！')
				this.nocheck = res.data.data
			},
		},
	}
</script>

<style scoped>
	.block {
		background-color: #FFFFFF;
		margin: 20px;
		padding: 20px;
		width: 1000px;


	}
</style>
