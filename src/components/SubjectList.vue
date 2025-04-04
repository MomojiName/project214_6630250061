<template>
  <div>
    <h4 class="mt-4">📘 รายละเอียดการเรียน</h4>

    <!-- ปุ่มเพิ่มวิชา -->
    <button class="btn btn-success mb-2" @click="showAdd = !showAdd">
      {{ showAdd ? "ปิดฟอร์มเพิ่มวิชา" : "เพิ่มวิชาใหม่" }}
    </button>

    <!-- ฟอร์มเพิ่ม -->
    <SubjectAdd v-if="showAdd" @added="getSubjects" />

    <!-- ตารางแสดงวิชา -->
    <table class="table table-bordered table-hover">
      <thead class="table-light">
        <tr>
          <th>รหัสวิชา</th>
          <th>ชื่อวิชา</th>
          <th>หน่วยกิต</th>
          <th>เกรด</th>
          <th>การจัดการ</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="subj in subjects" :key="subj.id">
          <td>{{ subj.id }}</td>
          <td>{{ subj.name }}</td>
          <td>{{ subj.credit }}</td>
          <td>{{ subj.grade }}</td>
          <td>
            <button class="btn btn-sm btn-warning" @click="editId = subj.id">
              แก้ไข
            </button>
            <button
              class="btn btn-sm btn-danger"
              @click="deleteSubject(subj.id)"
            >
              ลบ
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- ฟอร์มแก้ไข -->
    <SubjectEdit v-if="editId" :subjectId="editId" @updated="onUpdated" />
  </div>
</template>

<script>
import SubjectAdd from "./SubjectAdd.vue";
import SubjectEdit from "./SubjectEdit.vue";

export default {
  components: { SubjectAdd, SubjectEdit },
  data() {
    return {
      subjects: [],
      showAdd: false,
      editId: null,
    };
  },
  methods: {
    getSubjects() {
      fetch("http://localhost:3000/subjects")
        .then((res) => res.json())
        .then((data) => (this.subjects = data));
    },
    deleteSubject(id) {
      if (confirm("คุณแน่ใจหรือไม่ว่าต้องการลบวิชานี้?")) {
        fetch(`http://localhost:3000/subjects/${id}`, {
          method: "DELETE",
        }).then(() => this.getSubjects());
      }
    },
    onUpdated() {
      this.editId = null;
      this.getSubjects();
    },
  },
  mounted() {
    this.getSubjects();
  },
};
</script>

<style scoped>
table th,
table td {
  text-align: center;
  vertical-align: middle;
}

/* ปรับความกว้างให้แต่ละคอลัมน์ดูไม่เบียด */
table th:nth-child(1),
table td:nth-child(1) {
  min-width: 100px; /* รหัสวิชา */
}

table th:nth-child(2),
table td:nth-child(2) {
  min-width: 200px; /* ชื่อวิชา */
  text-align: left;
}

table th:nth-child(3),
table td:nth-child(3),
table th:nth-child(4),
table td:nth-child(4) {
  min-width: 80px; /* หน่วยกิต / เกรด */
}

table th:nth-child(5),
table td:nth-child(5) {
  min-width: 130px; /* ปุ่มจัดการ */
}
</style>
