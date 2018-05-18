<template>
    <div class="fillcontain">
        <head-top></head-top>
        <div >
            <section  v-show="true">
                <H3 align="center">查询</H3>
                <el-row style="margin-top: 20px;">
                    <el-col :span="12" :offset="4">
                <el-form :model="condition"  ref="condition" label-width="150px" class="demo-formData">
                    <el-form-item label="公证书号">
                        <el-input type="text"   v-model="condition.archiveNum">
                        </el-input>
                    </el-form-item>

                    <el-form-item label="当事人">
                        <el-input type="text"   v-model="condition.owner">
                        </el-input>
                    </el-form-item>

                    <el-form-item label="用卷人">
                        <el-input type="text"   v-model="condition.user">
                        </el-input>
                    </el-form-item>

                    <el-form-item  label="当前状态:">
                        <el-select :rows="1"  v-model="condition.status">
                            <el-option label="需求提交中"  value="0"></el-option>
                            <el-option label="卷宗已调走" value="1"></el-option>
                            <el-option label="已归还"   value="2"></el-option>
                        </el-select>
                    </el-form-item>

                    <el-form-item label="调档日期">
                        <el-date-picker v-model="condition.referdateStart" type="date" @change="dateChangebirthday" format="yyyy-MM-dd"
                                        value-format="yyyy-MM-dd" placeholder="选择日期">
                        </el-date-picker>
                          -
                        <el-date-picker v-model="condition.referdateEnd" type="date" @change="dateChangebirthday" format="yyyy-MM-dd"
                                        value-format="yyyy-MM-dd" placeholder="选择日期">
                        </el-date-picker>
                    </el-form-item>

                    <el-form-item label="归还日期">
                        <el-date-picker v-model="condition.returnStart" type="date" @change="dateChangebirthday" format="yyyy-MM-dd"
                                        value-format="yyyy-MM-dd" placeholder="选择日期">
                        </el-date-picker>
                        -
                        <el-date-picker v-model="condition.returnEnd" type="date" @change="dateChangebirthday" format="yyyy-MM-dd"
                                        value-format="yyyy-MM-dd" placeholder="选择日期">
                        </el-date-picker>
                    </el-form-item>

                    <el-form-item>
                        <el-button type="primary" @click="submitForm('condition')" class="submit_btn">查询</el-button>
                    </el-form-item>
                </el-form>
                    </el-col>
                    </el-row>
            </section>
        </div>

        <div class="table_container">
            <el-table
                :data="tableData"
                style="width: 100%">
                <el-table-column type="expand">
                  <template scope="props">
                    <el-form label-position="left" inline class="demo-table-expand">
                      <el-form-item label="调档日期">
                        <span>{{ props.row.referdate }}</span>
                      </el-form-item>
                      <el-form-item label="公证书号">
                        <span>{{ props.row.archiveNum }}</span>
                      </el-form-item>
                      <el-form-item label="当事人">
                        <span>{{ props.row.owner }}</span>
                      </el-form-item>
                      <el-form-item label="用卷人">
                        <span>{{ props.row.user }}</span>
                      </el-form-item>
                      <el-form-item label="当前状态">
                        <span>{{ props.row.status }}</span>
                      </el-form-item>
                      <el-form-item label="归还日期">
                        <span>{{ props.row.returnDate }}</span>
                      </el-form-item>
                    </el-form>
                  </template>
                </el-table-column>
                <el-table-column
                  label="调档日期"
                  prop="referdate">
                </el-table-column>
                <el-table-column
                  label="公证书号"
                  prop="archiveNum">
                </el-table-column>
                <el-table-column
                  label="当事人"
                  prop="owner">
                </el-table-column>
                <el-table-column
                    label="用卷人"
                    prop="user">
                </el-table-column>
                <el-table-column
                    label="当前状态"
                    prop="status">
                </el-table-column>
                <el-table-column
                    label="归还日期"
                    prop="returnDate">
                </el-table-column>
                <el-table-column label="操作" width="200">
                  <template scope="scope">
                    <el-button
                      size="mini"
                      @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                  </template>
                </el-table-column>
            </el-table>
            <div class="Pagination">
                <el-pagination
                  @size-change="handleSizeChange"
                  @current-change="handleCurrentChange"
                  :current-page="currentPage"
                  :page-size="20"
                  layout="total, prev, pager, next"
                  :total="count">
                </el-pagination>
            </div>
            <el-dialog title="更新卷宗信息" v-model="dialogFormVisible">
                <el-form :model="selectTable">
                    <el-form-item label="公证书号">
                        <el-input type="text" readonly="true"    v-model="selectTable.archiveNum">
                        </el-input>
                    </el-form-item>
                    <el-form-item label="当事人">
                        <el-input type="text"  readonly="true" v-model="selectTable.owner">
                        </el-input>
                    </el-form-item>

                    <el-form-item label="用卷人">
                        <el-input type="text"   v-model="selectTable.user">
                        </el-input>
                    </el-form-item>

                    <el-form-item  label="当前状态:">
                        <el-select :rows="1"  v-model="selectTable.status">
                            <el-option label="需求提交中"  value="0"></el-option>
                            <el-option label="卷宗已调走" value="1"></el-option>
                            <el-option label="已归还"   value="2"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item label="调档日期">
                        <el-date-picker v-model="selectTable.referdate" type="date" @change="dateChangebirthday" format="yyyy-MM-dd"
                                        value-format="yyyy-MM-dd" placeholder="选择日期">
                        </el-date-picker>
                    </el-form-item>
                    <el-form-item label="归还日期">
                        <el-date-picker v-model="selectTable.returndate" type="date" @change="dateChangebirthday" format="yyyy-MM-dd"
                                        value-format="yyyy-MM-dd" placeholder="选择日期">
                        </el-date-picker>
                    </el-form-item>
                </el-form>
              <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="updateShop">确 定</el-button>
              </div>
            </el-dialog>
        </div>
    </div>
</template>

<script>
    import headTop from '../components/headTop'
    import {baseUrl, baseImgPath} from '@/config/env'
    import {cityGuess, getArchives, getResturantsCount, foodCategory, updateArchive, searchplace, deleteResturant} from '@/api/getData'
    export default {
        data(){
            return {
                baseUrl,
                baseImgPath,
                city: {},
                offset: 0,
                limit: 20,
                count: 0,
                tableData: [],
                currentPage: 1,
                selectTable: {},
                dialogFormVisible: false,
                categoryOptions: [],
                selectedCategory: [],
                address: {},
                condition: {
                    referdateStart:'',
                    referdateEnd:'',
                    archiveNum: '',
                    owner: '',
                    user:'',
                    status:'',
                    returnStart:'',
                    returnEnd:''
                },
            }
        },
    	components: {
    		headTop,
    	},
        methods: {
            async getArchives() {
                const archiveData = await getArchives(this.condition);
                this.tableData = [];
                archiveData.forEach(item => {
                    const tableData = {};
                    tableData.referdate = item.referdate;
                    tableData.archiveNum = item.archiveNum;
                    tableData.owner = item.owner;
                    tableData.user = item.user;
                    tableData.status = item.status;
                    tableData.returndate = item.returndate;
                    this.tableData.push(tableData);
                })
            },
            async submitForm(condition) {
               this.getArchives();
            },
            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
            },
            handleCurrentChange(val) {
                this.currentPage = val;
                this.offset = (val - 1)*this.limit;
                this.getArchives();
            },
            handleEdit(index, row) {
                this.selectTable = row;
                this.address.address = row.address;
                this.dialogFormVisible = true;
                this.selectedCategory = row.category.split('/');
                if (!this.categoryOptions.length) {
                    this.getCategory();
                }
            },
            async updateShop(){
                this.dialogFormVisible = false;
                try{
                    const res = await updateArchive(this.selectTable)
                    if (res.status == 1) {
                        this.$message({
                            type: 'success',
                            message: '更新卷宗信息成功'
                        });
                        this.getResturants();
                    }else{
                        this.$message({
                            type: 'error',
                            message: res.message
                        });
                    }
                }catch(err){
                    console.log('更新卷宗信息失败', err);
                }
            },
        },
    }
</script>

<style lang="less">
	@import '../style/mixin';
    .demo-table-expand {
        font-size: 0;
    }
    .demo-table-expand label {
        width: 90px;
        color: #99a9bf;
    }
    .demo-table-expand .el-form-item {
        margin-right: 0;
        margin-bottom: 0;
        width: 50%;
    }
    .table_container{
        padding: 20px;
    }
    .Pagination{
        display: flex;
        justify-content: flex-start;
        margin-top: 8px;
    }
    .avatar-uploader .el-upload {
        border: 1px dashed #d9d9d9;
        border-radius: 6px;
        cursor: pointer;
        position: relative;
        overflow: hidden;
    }
    .avatar-uploader .el-upload:hover {
        border-color: #20a0ff;
    }
    .avatar-uploader-icon {
        font-size: 28px;
        color: #8c939d;
        width: 120px;
        height: 120px;
        line-height: 120px;
        text-align: center;
    }
    .avatar {
        width: 120px;
        height: 120px;
        display: block;
    }
</style>
