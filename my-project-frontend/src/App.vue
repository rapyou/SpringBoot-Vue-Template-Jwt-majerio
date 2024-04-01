<script setup>
import { useDark, useToggle } from '@vueuse/core'

useDark({
  selector: 'html',
  attribute: 'class',
  valueDark: 'dark',
  valueLight: 'light'
})

useDark({
  onChanged(dark) { useToggle(dark) }
})

</script>

<template>
  <el-container class="thin full-fill" id="main-container">
    <el-header class="main-header">
      <el-menu background-color="#222832" text-color="#fff" mode="horizontal" :router="true" 
        :default-active='create'>
        <el-menu-item index="/create"><el-icon><Odometer /></el-icon>创建任务</el-menu-item>
        <el-menu-item index="/manage"><el-icon><Odometer /></el-icon>任务管理</el-menu-item>
        <!-- <el-menu-item index="/map"><el-icon><Odometer /></el-icon>地图</el-menu-item> -->
      </el-menu>
    </el-header>
    <el-main class="main-main">
      <router-view />
    </el-main>
  </el-container>
</template>

<style scoped>
.main-container {
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;

}
.main-header{
  margin: 0;
  padding: 0;
}
.main-main{
  height: calc(100% - 60px);
  margin: 0;
  padding: 0;
}

header {
  line-height: 1.5;
}


.ful-fill{
  height: 100%;
  width: 100%;
}
.thin{
  margin: 0;
  padding: 0;
}
html{
  height: 100%;
  width: 100%;
}
body{
  height: 100%;
  width: 100%;
}
</style>
