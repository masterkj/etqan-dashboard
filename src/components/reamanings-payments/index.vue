<template>
  <div>
    <div class="va-row">
      <div class="flex xs12 md12">
        <vuestic-widget headerText="all subjects">
          <div class="table-responsive">
            <table class="table table-striped first-td-padding">
              <thead>
                <tr>
                  <td>{{'cours-name' | translate}}</td>
                  <td>{{'have-exam' | translate}}</td>
                  <td>{{'practical-mark' | translate}}</td>
                  <td></td>
                </tr>
              </thead>
              <tbody>
                <tr v-for="subject in subjects" :key="subject.id">
                  <td>{{subject.cours_name}}</td>
                  <td>{{haveExam(subject.have_exam)}}</td>
                  <td>{{subject.Practical_mark}}</td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>

          <button class="btn btn-success" @click="showStaticModal()">{{'add' | translate }}</button>
        </vuestic-widget>
      </div>
    </div>
    <vuestic-modal
      :show.sync="show"
      v-bind:large="true"
      v-bind:force="true"
      ref="staticModal"
      :cancelClass="'none'"
      :okText="'modal.close' | translate"
    >
      <div slot="title">{{'add a sbject' | translate}}</div>
      <div>
        <form>
          <div class="va-row">
            <div class="flex md4">
              <fieldset>
                <div class="form-group">
                  <div class="input-group">
                    <input id="simple-input" v-model="newSubject.courseName" required>
                    <label class="control-label" for="simple-input">
                      {{'course-name'
                      | translate}}
                    </label>
                    <i class="bar"></i>
                  </div>
                </div>

                <div class="form-group">
                  <div class="input-group">
                    <textarea
                      type="text"
                      v-model="newSubject.description"
                      id="simple-textarea"
                      required
                    ></textarea>
                    <label class="control-label" for="simple-textarea">
                      {{'course-description'
                      | translate}}
                    </label>
                    <i class="bar"></i>
                  </div>
                </div>
              </fieldset>
            </div>

            <div class="flex md4">
              <fieldset>
                <div class="form-group">
                  <div class="input-group">
                    <input id="simple-input" v-model="newSubject.courseMark" required>
                    <label class="control-label" for="simple-input">
                      {{'course-mark'
                      | translate}}
                    </label>
                    <i class="bar"></i>
                  </div>
                </div>

                <div class="form-group">
                  <div class="input-group">
                    <input
                      :disabled="!newSubject.isPractical"
                      v-model="newSubject.practicalMark"
                      id="simple-input"
                      required
                    >
                    <label class="control-label" for="simple-input">
                      {{'practical-mark'
                      | translate}}
                    </label>
                    <i class="bar"></i>
                  </div>
                </div>
              </fieldset>
            </div>

            <div class="flex md4">
              <fieldset>
                <div class="form-group">
                  <vuestic-switch v-model="newSubject.isPractical">
                    <span slot="trueTitle">{{'practical' | translate}}</span>
                    <span slot="falseTitle">{{"not" | translate}}</span>
                  </vuestic-switch>
                </div>
                <div class="form-group">
                  <div class="input-group">
                    <input id="simple-input" v-model="newSubject.categoryId" required>
                    <label class="control-label" for="simple-input">
                      {{'category-id'
                      | translate}}
                    </label>
                    <i class="bar"></i>
                  </div>
                </div>
                <div class="form-group">
                  <button
                    class="btn btn-success"
                    @click="submitNewSubject()"
                  >{{'submit' | translate }}</button>
                </div>
              </fieldset>
            </div>
          </div>
        </form>
      </div>
    </vuestic-modal>
  </div>
</template>

<script>
export default {
  computed: {
    isSuccessfulEmailValid () {
      let isValid = false
      if (this.formFields.successfulEmail) {
        isValid =
          this.formFields.successfulEmail.validated &&
          this.formFields.successfulEmail.valid
      }
      return isValid
    }
  },

  methods: {
    showStaticModal () {
      this.$refs.staticModal.open()
    },
    submitNewSubject () {},
    haveExam (num) {
      if (num === 0) return false
      else return true
    }
  },
  name: 'Subjects',
  data () {
    return {
      show: true,

      newSubject: {
        isPractical: true,
        courseName: '',
        description: '',
        practicalMark: null,
        courseMark: null,
        categoryid: 0
      },

      subjects: [
        {
          id: '1',
          fullname: 'ALETQAN',
          shortname: 'etqan',
          displayname: 'ALETQAN',
          categoryid: '0',
          summary: 'welcome!!</p>',
          summaryformat: '1',
          format: 'site',
          startdate: '0',
          enddate: '0',
          numsections: 1,
          categorysortorder: '1',
          idnumber: '',
          showgrades: '1',
          showreports: '0',
          newsitems: '3',
          visible: '1',
          maxbytes: '0',
          groupmode: '0',
          groupmodeforce: '0',
          defaultgroupingid: '0',
          lang: '',
          timecreated: '1557059677',
          timemodified: '1557062764',
          forcetheme: '',
          enablecompletion: '0',
          completionnotify: '0',
          courseformatoptions: [
            {
              name: 'numsections',
              value: 1
            }
          ],
          cours_name: 'compilar17',
          have_exam: '0',
          Practical_mark: '130'
        }
      ]
    }
  },
  created () {
    this.$nextTick(() => {
      this.$validator.validateAll()
    })
  }
}
</script>

<style lang="scss" scoped>
.right {
  float: right;
}
.color-icon-label-table {
  td:first-child {
    width: 1rem;
  }
}

.modals-page {
  .modals-list {
    .btn {
      margin-right: 20px;
      margin-bottom: 25px;
    }
  }
}
</style>
