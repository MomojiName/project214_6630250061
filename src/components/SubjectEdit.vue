<template>
    <div class="card p-3 mb-3">
      <h5>แก้ไขวิชา</h5>
      <form @submit.prevent="handleSubmit">
        <div class="row">
          <div class="col-md-3">
            <input v-model="subject.id" class="form-control" readonly />
          </div>
          <div class="col-md-4">
            <input v-model="subject.name" class="form-control" required />
          </div>
          <div class="col-md-2">
            <input v-model.number="subject.credit" type="number" class="form-control" required />
          </div>
          <div class="col-md-2">
            <input v-model="subject.grade" class="form-control" required />
          </div>
          <div class="col-md-1">
            <button type="submit" class="btn btn-success">บันทึก</button>
          </div>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    props: ['subjectId'],
    data() {
      return {
        subject: {}
      }
    },
    methods: {
      handleSubmit() {
        fetch(`http://localhost:3000/subjects/${this.subjectId}`, {
          method: 'PATCH',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(this.subject)
        }).then(() => this.$emit('updated'))
      }
    },
    mounted() {
      fetch(`http://localhost:3000/subjects/${this.subjectId}`)
        .then(res => res.json())
        .then(data => this.subject = data)
    }
  }
  </script>
  