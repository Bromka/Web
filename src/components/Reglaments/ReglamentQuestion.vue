
<template>
  <BoardModalBoxDelete
    v-if="showDeleteQuestion"
    title="Удалить вопрос"
    text="Вы действительно хотите удалить вопрос?"
    @cancel="showDeleteQuestion = false"
    @yes="deleteQuestion"
  />
  <div class="flex justify-center">
    <div
      v-if="isEditing ? true: rightAnswersAmount(question)"
      class="bg-white p-3 rounded-[10px] mb-2 w-[580px]"
    >
      <div class="flex justify-between w-[550px] mb-2">
        <span class="text-[#424242] font-[700] ml-2 text-[15px]">Текст вопроса</span>
        <div
          v-if="isEditing && canEdit"
          class="flex flex-row items-center cursor-pointer mr-1 hover:transition hover:opacity-[0.8]"
          @click="showDeleteQuestion = true"
        >
          <svg
            width="14"
            height="16"
            viewBox="0 0 14 16"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M5.92308 2.64286H8.07692C8.07692 2.3587 7.96346 2.08617 7.7615 1.88524C7.55954 1.68431 7.28562 1.57143 7 1.57143C6.71438 1.57143 6.44046 1.68431 6.2385 1.88524C6.03654 2.08617 5.92308 2.3587 5.92308 2.64286ZM4.84615 2.64286C4.84615 2.07454 5.07308 1.52949 5.477 1.12763C5.88093 0.725765 6.42876 0.5 7 0.5C7.57124 0.5 8.11907 0.725765 8.523 1.12763C8.92692 1.52949 9.15385 2.07454 9.15385 2.64286H13.4615C13.6043 2.64286 13.7413 2.6993 13.8423 2.79976C13.9433 2.90023 14 3.03649 14 3.17857C14 3.32065 13.9433 3.45691 13.8423 3.55738C13.7413 3.65784 13.6043 3.71429 13.4615 3.71429H12.8542L11.5565 13.1836C11.4684 13.8253 11.1497 14.4135 10.6593 14.8394C10.1688 15.2653 9.5399 15.5 8.88892 15.5H5.11108C4.4601 15.5 3.83116 15.2653 3.34072 14.8394C2.85028 14.4135 2.53155 13.8253 2.44354 13.1836L1.14585 3.71429H0.538462C0.395653 3.71429 0.258693 3.65784 0.157712 3.55738C0.0567306 3.45691 0 3.32065 0 3.17857C0 3.03649 0.0567306 2.90023 0.157712 2.79976C0.258693 2.6993 0.395653 2.64286 0.538462 2.64286H4.84615ZM5.92308 6.39286C5.92308 6.25078 5.86635 6.11452 5.76537 6.01405C5.66438 5.91358 5.52742 5.85714 5.38462 5.85714C5.24181 5.85714 5.10485 5.91358 5.00387 6.01405C4.90288 6.11452 4.84615 6.25078 4.84615 6.39286V11.75C4.84615 11.8921 4.90288 12.0283 5.00387 12.1288C5.10485 12.2293 5.24181 12.2857 5.38462 12.2857C5.52742 12.2857 5.66438 12.2293 5.76537 12.1288C5.86635 12.0283 5.92308 11.8921 5.92308 11.75V6.39286ZM8.61539 5.85714C8.75819 5.85714 8.89515 5.91358 8.99614 6.01405C9.09712 6.11452 9.15385 6.25078 9.15385 6.39286V11.75C9.15385 11.8921 9.09712 12.0283 8.99614 12.1288C8.89515 12.2293 8.75819 12.2857 8.61539 12.2857C8.47258 12.2857 8.33562 12.2293 8.23463 12.1288C8.13365 12.0283 8.07692 11.8921 8.07692 11.75V6.39286C8.07692 6.25078 8.13365 6.11452 8.23463 6.01405C8.33562 5.91358 8.47258 5.85714 8.61539 5.85714ZM3.51077 13.0389C3.56362 13.4239 3.75485 13.7768 4.04906 14.0322C4.34328 14.2877 4.72056 14.4285 5.11108 14.4286H8.88892C9.27963 14.4288 9.65717 14.2881 9.95161 14.0326C10.246 13.7771 10.4374 13.4241 10.4903 13.0389L11.7675 3.71429H2.23246L3.51077 13.0389Z"
              fill="#7E7E80"
            />
          </svg>
          <span class="ml-[5px] text-[12px] font-[400] text-[#4C4C4D] font-roboto">
            Удалить
          </span>
        </div>
      </div>
      <div class="px-1 flex justify-between items-center group">
        <div class="bg-[#F4F5F7] rounded-[6px] min-h-[81px] min-w-[550px]">
          <div
            :ref="question.uid + 'input'"
            placeholder="Текст вопроса"
            class="font-[500] text-[14px] mx-4 mt-4 leading-6 min-h-[60px]"
            :contenteditable="isEditing && canEdit"
            @blur="changeQuestionName($event)"
            @keydown.enter.exact.prevent="$emit('addQuestion')"
            v-text="question.name"
          />
          <div class="flex justify-end items-end pb-2 pr-2">
            <svg
              class="right-0"
              width="11"
              height="11"
              viewBox="0 0 11 11"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                fill-rule="evenodd"
                clip-rule="evenodd"
                d="M9.5294 1.52941L1.5294 9.52935L0.46875 8.46869L8.46875 0.46875L9.5294 1.52941ZM10.5294 5.52935L5.52941 10.5294L4.46875 9.46869L9.46875 4.46869L10.5294 5.52935Z"
                fill="#8C8CA2"
              />
            </svg>
          </div>
        </div>
      </div>
      <div class="mt-4 mb-2">
        <span
          v-if="rightAnswersAmount(question) === 1"
          class="text-[#424242] font-[700] ml-2 text-[15px]"
        >
          В данном вопросе один правильный ответ.
        </span>
        <span
          v-if="rightAnswersAmount(question) > 1"
          class="text-[#424242] font-[700] ml-2 text-[15px]"
        >
          В данном вопросе более одного правильного ответа.
        </span>
        <span
          v-if="rightAnswersAmount(question) === 0"
          class="text-[#424242] font-[700] ml-2 text-[15px]"
        >
          Добавьте вариант ответа.
        </span>
      </div>
      <template
        v-for="(answer) in question.answers"
        :key="answer.uid"
      >
        <ReglamentAnswer
          :ref="answer.uid"
          class="mb-1"
          :is-editing="isEditing"
          :answer="answer"
          @set-right-answer="setRightAnswer"
          @onSelectAnswer="onSelectAnswer"
          @addAnswer="onAddAnswer"
          @deleteAnswer="onDeleteAnswer"
          @update-answer-name="updateAnswerName"
        />
      </template>
      <div
        v-if="canEdit && isEditing"
        class="flex flex-row min-w-[550px] items-center justify-center ml-[5px] mt-2 font-[300] text-[14px] bg-[#F4F5F7] p-1 px-3 rounded-[6px] hover:cursor-pointer transition hover:opacity-[0.8]"
        @click.stop="onAddAnswer"
      >
        <svg
          width="13"
          height="12"
          viewBox="0 0 13 12"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M7.20474 5.29517L7.20943 1.00347C7.20941 0.815309 7.13466 0.634852 7.0016 0.501794C6.86854 0.368736 6.68808 0.293977 6.49992 0.293964C6.31176 0.29395 6.13132 0.368683 5.99828 0.501722C5.86524 0.634761 5.79051 0.815208 5.79052 1.00337L5.79582 5.29507L1.50412 5.28976C1.31596 5.28975 1.13552 5.36448 1.00248 5.49752C0.869438 5.63056 0.794705 5.81101 0.794718 5.99917C0.794732 6.18733 0.86949 6.36779 1.00255 6.50084C1.13561 6.6339 1.31606 6.70866 1.50422 6.70867L5.79592 6.70398L5.79123 10.9957C5.79085 11.089 5.80895 11.1814 5.84447 11.2676C5.88 11.3539 5.93225 11.4322 5.99821 11.4982C6.06417 11.5642 6.14253 11.6164 6.22878 11.6519C6.31503 11.6875 6.40746 11.7056 6.50073 11.7052C6.59401 11.7056 6.68643 11.6875 6.77268 11.652C6.85892 11.6165 6.93728 11.5642 7.00323 11.4983C7.06918 11.4323 7.12142 11.354 7.15693 11.2677C7.19244 11.1815 7.21053 11.0891 7.21014 10.9958L7.20484 6.70408L11.4965 6.70938C11.5898 6.70977 11.6822 6.69169 11.7685 6.65618C11.8547 6.62067 11.9331 6.56843 11.999 6.50248C12.065 6.43653 12.1172 6.35817 12.1527 6.27192C12.1882 6.18568 12.2063 6.09325 12.2059 5.99998C12.2063 5.90671 12.1882 5.81428 12.1527 5.72803C12.1172 5.64178 12.0649 5.56341 11.999 5.49745C11.933 5.43149 11.8546 5.37924 11.7684 5.34372C11.6821 5.30819 11.5897 5.2901 11.4964 5.29048L7.20474 5.29517Z"
            fill="#4C4C4D"
          />
        </svg>
        <span class="ml-2 text-[13px] font-[500] text-[#4C4C4D] font-roboto">Добавить еще ответ</span>
      </div>
    </div>
  </div>
</template>
<script>
import ReglamentAnswer from './ReglamentAnswer.vue'
import BoardModalBoxDelete from '@/components/Board/BoardModalBoxDelete.vue'
export default {
  components: {
    ReglamentAnswer,
    BoardModalBoxDelete
  },
  props: {
    question: {
      type: Object,
      default: () => ({})
    },
    isEditing: {
      type: Boolean,
      default: false
    },
    reglament: {
      type: Object,
      default: () => ({})
    }
  },
  emits: ['deleteQuestion', 'deleteAnswer', 'setRightAnswer', 'addQuestion', 'pushAnswer', 'selectAnswer', 'updateAnswerName', 'updateQuestionName'],
  expose: ['onFocus'],
  data () {
    return {
      showDeleteQuestion: false,
      showDeleteAnswer: false,
      rightAnswer: false
    }
  },
  computed: {
    canEdit () {
      const userType = this.$store.state.employees.employees[this.$store.state.user.user.current_user_uid].type
      return (this.$store.state.greedSource?.email_creator === this.$store.state.user.user.current_user_email) || (this.editorsCanEdit) || (userType === 2 || userType === 1)
    },
    editorsCanEdit () {
      return this.reglament.editors.includes(this.$store.state.user.user.current_user_email)
    }
  },
  methods: {
    gotoNode (uid) {
      this.$refs[uid][0].onFocus()
    },
    questionPlaceholder (question) {
      if (question.name === '' && question.invalid) {
        return 'Поле вопроса не должно быть пустым'
      }
      return this.isEditing && this.canEdit ? 'Текст вопроса' : ''
    },
    rightAnswersAmount (question) {
      let count = 0
      for (let i = 0; i < question.answers.length; i++) {
        if (question.answers[i].is_right) {
          count++
        }
      }
      return count
    },
    onAddAnswer () {
      const data = {
        name: '',
        uid: this.uuidv4(),
        uid_question: this.question.uid,
        is_right: false,
        needToCreate: true,
        needToUpdate: false
      }
      this.$emit('pushAnswer', data)
      this.$nextTick(() => {
        this.gotoNode(data.uid)
      })
    },
    setRightAnswer (answerParams) {
      const [answer, isRight] = answerParams
      console.log(answer)
      const data = {
        uid: answer.uid,
        uid_question: this.question.uid,
        name: answer.name,
        is_right: isRight,
        needToCreate: answer.needToCreate,
        needToUpdate: true
      }

      this.$emit('setRightAnswer', data)
    },
    onSelectAnswer (uid) {
      for (let i = 0; i < this.question.answers.length; i++) {
        if (this.question.answers[i].uid === uid) {
          if (this.question.answers[i].selected) {
            this.$emit('selectAnswer', [this.question.answers[i], false])
            return
          }
          console.log(this.question)
          this.$emit('selectAnswer', [this.question.answers[i], true])
          return
        }
      }
    },
    uuidv4 () {
      return ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, (c) =>
        (
          c ^
          (crypto.getRandomValues(new Uint8Array(1))[0] & (15 >> (c / 4)))
        ).toString(16)
      )
    },
    onDeleteAnswer (uid) {
      this.$emit('deleteAnswer', uid)
    },
    updateAnswerName (resp) {
      const [name, answer] = resp
      console.log(name, answer)
      const data = {
        uid: answer.uid,
        uid_question: this.question.uid,
        name: name,
        is_right: answer.is_right,
        needToCreate: answer.needToCreate,
        needToUpdate: answer.needToUpdate,
        invalid: name === ''
      }
      this.$emit('updateAnswerName', data)
    },
    deleteQuestion () {
      this.showDeleteQuestion = false
      this.$emit('deleteQuestion', this.question.uid)
    },
    changeQuestionName (event) {
      const data = {
        uid: this.question.uid,
        name: event.target.innerText,
        needToCreate: this.question.needToCreate,
        needToUpdate: true,
        invalid: event.target.innerText === ''
      }
      this.$emit('updateQuestionName', data)
    },
    onFocus () {
      this.$refs[this.question.uid + 'input'].focus()
      const range = document.createRange()
      const sel = document.getSelection()
      range.setStart(this.$refs[this.question.uid + 'input'], 0)
      range.collapse(true)
      sel.removeAllRanges()
      sel.addRange(range)
    }
  }
}
</script>
<style scoped>
[placeholder]:empty::before {
    content: attr(placeholder);
    color: #555;
}
.invalid[placeholder]:empty::before {
    color: rgb(239 68 68);
}
</style>
