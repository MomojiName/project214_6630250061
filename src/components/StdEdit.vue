<template>
  <!-- Formบันทึกข้อมูล -->
  <div class="card" style="background-color: #fffdf0">
    <div class="card-body">
      <form @submit.prevent="handleSubmit()">
        <div class="row">
          <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
            <label for="stdId" class="form-label">รหัสนิสิต</label>
            <input
              type="text"
              id="stdId"
              class="form-control"
              v-model.trim="std.id"
            />
          </div>
          <div class="col-lg-8 col-md-8 col-sm-6 mt-2">
            <label for="stdName" class="form-label">ชื่อ นามสกุล</label>
            <input
              type="text"
              id="stdName"
              class="form-control"
              v-model.trim="std.name"
            />
          </div>

          <!-- ✅ เพิ่ม input โรงเรียนเดิม -->
          <div class="col-lg-12 col-md-12 col-sm-12 mt-2">
            <label for="stdSchool" class="form-label">โรงเรียนเดิม</label>
            <input
              type="text"
              id="stdSchool"
              class="form-control"
              v-model.trim="std.school"
            />
          </div>

          <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
            <label class="form-label">สาขา</label>
            <div class="form-check">
              <input
                class="form-check-input"
                type="radio"
                id="stdMajor"
                value="CS"
                v-model="std.major"
              />
              <label class="form-check-label" for="stdMajor">
                Computer Science
              </label>
            </div>
            <div class="form-check">
              <input
                class="form-check-input"
                type="radio"
                id="stdMajor"
                value="DT"
                v-model="std.major"
              />
              <label class="form-check-label" for="stdMajor">
                Digital Technology
              </label>
            </div>
          </div>

          <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
            <label for="stdYr" class="form-label">ชั้นปี</label>
            <select id="stdYr" class="form-select" v-model="std.yr">
              <option value="1">ปี1</option>
              <option value="2">ปี2</option>
              <option value="3">ปี3</option>
              <option value="4">ปี4</option>
              <option value="5">เกินปี4</option>
            </select>
          </div>

          <div class="col-2 mt-5">
            <button type="submit" class="btn btn-warning">บันทึก</button>
          </div>
        </div>

        <div class="alert alert-success mt-3" v-if="editOK">
          {{ editMessage }}
        </div>
        <div class="alert alert-danger mt-3" v-if="editErr">
          {{ editMessage }}
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import { EventBus } from '../event-bus';

export default {
  name: "StdEdit",
  props: ["stdId"],
  data() {
    return {
      std: {
        id: null,
        name: null,
        major: null,
        yr: 2,
        isActive: false,
        school: null // ✅ เพิ่ม field school
      },
      editOK: false,
      editErr: false,
      editMessage: null
    };
  },
  methods: {
    handleSubmit() {
      fetch(`http://localhost:3000/student`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(this.std)
      })
        .then(() => {
          this.editOK = true;
          this.editErr = false;
          this.editMessage = "สำเร็จ";
          EventBus.emit("stdChange", { message: "edit" });
        })
        .catch((err) => {
          this.editErr = true;
          this.editOK = false;
          this.editMessage = err;
        });
    }
  },
  mounted() {
    fetch(`http://localhost:3000/student`)
      .then((res) => res.json())
      .then((data) => (this.std = data))
      .catch((err) => console.log(err.message));
  }
};
</script>

<style></style>
