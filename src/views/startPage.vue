<template>
  <div class="mainContainer" :style="{
    'flex-direction': `${mobile? 'column':'row'}`
  }">
    <nav-bar ref="navBarRef"/>
    <el-drawer z-index="1" :with-header="false" :modal-append-to-body="false" class="drawer" before-close="forbiddenClose" v-model="showPage" :size="mobile? '95%':'65%'" :modal="false" :show-close="false">
      <div class="cardWrap" :style="{
      'padding-top': `${mobile? 20:10}vh`
      }">
        <div class="card" :style="{
        'width': `${mobile? '95%':'65vw'}`
        }">
          <input-module/>
          <div>
            <el-button @click="openSetting" class="settingButton"><el-icon><Tools /></el-icon></el-button>
          </div>
        </div>
      </div>
    </el-drawer>
  </div>
  <el-dialog v-model="showSetting" title="设置">
    <el-form :model="settingForm">
      <el-form-item label="刷新率(每秒刷新次数)">
        <el-input v-model="settingForm.fps"/>
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="showSetting = false">取消</el-button>
        <el-button type="primary" @click="saveSetting">
          保存
        </el-button>
      </span>
    </template>
  </el-dialog>
</template>

<script lang="ts" setup>
import NavBar from "@/components/NavBar.vue";
import InputModule from "@/components/InputModule.vue";
import {cycle, fps, mobile, showPage} from "@/global/global";
import {reactive, ref} from "vue";
import {ElMessage} from "element-plus";

const forbiddenClose = (done: () => void) => {}

const showSetting = ref(false);

const settingForm = reactive({
  fps: 0,
})

const openSetting = () => {
  showSetting.value = true;
  settingForm.fps = fps.value;
}

const saveSetting = () => {
  try{
    let a = Number(settingForm.fps)
  }catch (e){
    ElMessage({
      message: '输入必须为数字',
      type: 'error'
    })
    openSetting();
    return;
  }
  fps.value = settingForm.fps;
  showSetting.value = false;
  ElMessage({
    message: '设置成功',
    type: 'success'
  })
}
</script>

<style scoped>
.mainContainer{
  display: flex;
  flex-direction: row;
  width: 100vw;
  height: 100vh;
}
/deep/.el-drawer.rtl {
  background: rgba(0,0,0,0);
  box-shadow: rgba(0,0,0,0) 0 0 0;
  margin: 0 -20px;
}
.settingButton{
  background-color: rgba(0,0,0,0);
  color: #FFF;
  border-style: none;
  font-size: 4vh;
}
</style>
