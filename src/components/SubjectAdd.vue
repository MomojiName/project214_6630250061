<template>
    <form @submit.prevent="handleSubmit" class="mb-4">
      <!-- แถวของ input -->
      <div class="row mb-2">
        <div class="col-md-3">
          <input v-model="subject.id" class="form-control" placeholder="รหัสวิชา" required />
        </div>
        <div class="col-md-4">
          <input v-model="subject.name" class="form-control" placeholder="ชื่อวิชา" required />
        </div>
        <div class="col-md-2">
          <input v-model.number="subject.credit" type="number" class="form-control" placeholder="หน่วยกิต" required />
        </div>
        <div class="col-md-2">
          <input v-model="subject.grade" class="form-control" placeholder="เกรด" required />
        </div>
      </div>
  
      <!-- แถวปุ่มบันทึก ขยับไปอยู่บรรทัดล่างและชิดขวาสุด -->
      <div class="row">
        <div class="col text-end">
          <button type="submit" class="btn btn-primary">บันทึก</button>
        </div>
      </div>
    </form>
  </template>
  
  <script>
  export default {
    data() {
      return {
        subject: {
          id: "",
          name: "",
          credit: 0,
          grade: "",
        },
      };
    },
    methods: {
      handleSubmit() {
        fetch("http://localhost:3000/subjects", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(this.subject),
        }).then(() => {
          this.$emit("added");
          this.subject = { id: "", name: "", credit: 0, grade: "" };
        });
      },
    },
  };
  </script>
  
  <style scoped>
  form input {
    font-size: 14px;
    padding: 8px;
  }
  
  .row > div {
    margin-bottom: 10px;
  }
  </style>
  