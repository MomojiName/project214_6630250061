<template>
    <div class="card" style="width: 24rem;">
      <!-- รูปนิสิต -->
      <img
        :src="student.photo"
        class="card-img-top"
        alt="student photo"
      />
      <div class="card-body">
        <h5 class="card-title">{{ student.name }}</h5>
        <p class="card-text">
          รหัสนิสิต : {{ student.id }}<br />
          สาขา : {{ student.major }}<br />
          ชั้นปีที่ : {{ student.yr }}<br />
          โรงเรียนเดิม : {{ student.school }}
        </p>
  
        <!-- ปุ่มแก้ไข -->
        <button class="btn btn-warning" @click="editMode = !editMode">
          {{ editMode ? 'ยกเลิก' : 'แก้ไขข้อมูล' }}
        </button>
  
        <!-- แบบฟอร์มแก้ไข -->
        <div v-if="editMode" class="mt-3">
          <StdEdit :stdId="student.id" />
        </div>
      </div>
  
      <!-- ปุ่มแสดงรายวิชา -->
      <button class="btn btn-secondary mt-3" @click="showSubjects = !showSubjects">
        {{ showSubjects ? 'ซ่อนข้อมูลรายวิชา' : 'แสดงข้อมูลรายวิชา' }}
      </button>
  
      <!-- แสดงรายวิชา -->
      <SubjectList v-if="showSubjects" />
    </div>
  </template>
  
  <script>
  import { EventBus } from '@/event-bus';
  import StdEdit from './StdEdit.vue';
  import SubjectList from './SubjectList.vue';
  
  export default {
    name: 'StdList',
    components: {
      StdEdit,
      SubjectList
    },
    data() {
      return {
        student: {},
        editMode: false,
        showSubjects: false
      };
    },
    methods: {
      getData() {
        fetch('http://localhost:3000/student')
          .then((res) => res.json())
          .then((data) => {
            console.log("✅ ได้ข้อมูล:", data);
            this.student = data;
  
            // ✅ ส่งชื่อและรูปไปให้ App.vue
            EventBus.emit('std_profile', {
              name: data.name,
              photo: data.photo
            });
          })
          .catch((err) => console.log("❌ ดึงข้อมูลไม่สำเร็จ:", err.message));
      }
    },
    mounted() {
      this.getData();
  
      EventBus.on('stdChange', (data) => {
        console.log(`Student Change --> ${data.message}`);
        this.getData();
      });
    }
  };
  </script>
  
  <style scoped>
  .card-img-top {
    max-height: 300px;
    object-fit: cover;
  }
  </style>
  