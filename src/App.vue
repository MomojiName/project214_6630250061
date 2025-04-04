<template>
  <div class="background-container">
    <div class="container d-flex justify-content-center align-items-center vh-100">
      <div class="card shadow-lg p-5 text-center">
        <!-- ✅ รูปโปรไฟล์ -->
        <div class="profile-box mx-auto">
          <img :src="profilePhoto" alt="Profile" class="profile-logo" />
          <div class="profile-name mt-3">{{ profileName }}</div>
        </div>

        <h1 class="mt-4">นิสิตคณะวิทยาศาสตร์ ศรีราชา</h1>

        <button type="button" class="btn btn-secondary my-4" @click="showList = !showList">
          {{ showList ? "ซ่อนข้อมูลนิสิต" : "แสดงข้อมูลนิสิต" }}
        </button>

        <transition name="fade">
          <div v-if="showList">
            <StdList />
          </div>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
import StdList from "./components/StdList.vue";
import { EventBus } from "./event-bus";

export default {
  name: "App",
  components: {
    StdList,
  },
  data() {
    return {
      showList: false,
      stdId: null,
      stdMajor: null,
    };
  },
  computed: {
    profileName() {
      return "Chonlasit Singrueang";
    },
    profilePhoto() {
      return "IMG_2500.jpg";
    },
  },
  mounted() {
    EventBus.on("std_select", (data) => {
      this.stdId = data.id;
      this.stdMajor = data.major;
    });
  },
};
</script>

<style scoped>
/* ✅ พื้นหลังแบบเต็มจอ */
.background-container {
  width: 100vw;
  height: 100vh;
  background-image: url("anime-character-traveling_23-2151140116.avif"); /* 🎨 เปลี่ยน URL เป็นภาพพื้นหลังที่ต้องการ */
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  display: flex;
  align-items: center;
  justify-content: center;
}


/* ✅ ปรับขนาดของรูปโปรไฟล์ */

.profile-logo {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  border: 4px solid #ccc;
  object-fit: cover;
  box-shadow: 0 0 12px rgba(0, 0, 0, 0.2);
}

.profile-name {
  font-size: 18px;
  font-weight: bold;
  color: #333;
}

/* ✅ เพิ่มเอฟเฟกต์ fade-in/out */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>