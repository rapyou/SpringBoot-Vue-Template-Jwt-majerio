<template>
    <el-container class="thin ful-fill">
        <el-aside width="420px" class="thin">
            <el-steps style="max-width: 610px;margin: 35px 30px 35px 130px;height:85%" :active="1" direction="vertical">
                <el-step title="算法" :icon="Operation" />
                <el-step title="输入影像" :icon="Picture" />
                <el-step title="计算范围" :icon="Place" />
            </el-steps>
        </el-aside>
        <el-main class="thin">
            <el-container class="full-fill">
                <el-main class="thin">
                    <el-select v-model="algorithm" clearable placeholder="请选择算法" style="width: 340px">
                        <el-option v-for="item in algorithmList" :key="item.value" :label="item.label" :value="item.value" />
                    </el-select>
                </el-main>
                <el-footer class="thin" height="50px" style="background-color: aliceblue;"></el-footer>
            </el-container>
        </el-main>
    </el-container>
</template>

<script setup>
import {ref} from 'vue'
import { Operation, Picture, Place } from '@element-plus/icons-vue'
const algorithm = ref('')
const algorithmList = [{
    value: 'NDVI'
},{
    value: 'EVI'
},{
    value: 'LAI'
}]
</script>
<style scoped></style>
