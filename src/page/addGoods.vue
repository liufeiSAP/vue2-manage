<template>
    <div>
        <head-top></head-top>
        <div >
            <section  v-show="true">
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

                            </el-form-item>

                            <el-form-item label="归还日期">
                                <el-date-picker v-model="condition.returnStart" type="date" @change="dateChangebirthday" format="yyyy-MM-dd"
                                                value-format="yyyy-MM-dd" placeholder="选择日期">
                                </el-date-picker>
                            </el-form-item>

                            <el-form-item>
                                <el-button type="primary" @click="submitForm('condition')" class="submit_btn">增加</el-button>
                            </el-form-item>
                        </el-form>
                    </el-col>
                </el-row>

            </section>
         </div>
    </div>
</template>

<script>
 	import headTop from '@/components/headTop'
    import {getCategory, addCategory, addFood} from '@/api/getData'
    import {baseUrl, baseImgPath} from '@/config/env'
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
    	created(){
    	},
    	computed: {
    	},
    	methods: {
    		async initData(){
    		},
		},
    }
</script>

<style lang="less">
	@import '../style/mixin';
	.form{
		min-width: 400px;
		margin-bottom: 30px;
		&:hover{
			box-shadow: 0 0 8px 0 rgba(232,237,250,.6), 0 2px 4px 0 rgba(232,237,250,.5);
			border-radius: 6px;
			transition: all 400ms;
		}
	}
	.food_form{
		border: 1px solid #eaeefb;
		padding: 10px 10px 0;
	}
	.form_header{
		text-align: center;
		margin-bottom: 10px;
	}
	.category_select{
		border: 1px solid #eaeefb;
		padding: 10px 30px 10px 10px;
		border-top-right-radius: 6px;
		border-top-left-radius: 6px;
	}
	.add_category_row{
		height: 0;
		overflow: hidden;
		transition: all 400ms;
		background: #f9fafc;
	}
	.showEdit{
		height: 185px;
	}
	.add_category{
		background: #f9fafc;
		padding: 10px 30px 0px 10px;
		border: 1px solid #eaeefb;
		border-top: none;
	}
	.add_category_button{
		text-align: center;
		line-height: 40px;
		border-bottom-right-radius: 6px;
		border-bottom-left-radius: 6px;
		border: 1px solid #eaeefb;
		border-top: none;
		transition: all 400ms;
		&:hover{
			background: #f9fafc;
			span, .edit_icon{
				color: #20a0ff;
			}
		}
		span{
			.sc(14px, #999);
			transition: all 400ms;
		}
		.edit_icon{
			color: #ccc;
			transition: all 400ms;
		}
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
	.cell{
		text-align: center;
	}
</style>
