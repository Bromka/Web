
<template>
  <BoardModalBoxRename
    v-if="showEditname"
    :show="showEditname"
    title="Введите новое имя пользователя"
    :value="userName()"
    @cancel="showEditname = false"
    @save="changeUserName"
  />
  <BoardModalBoxRename
    v-if="showEditphone"
    :show="showEditphone"
    title="Введите новый номер телефона"
    :value="userPhone()"
    @cancel="showEditphone = false"
    @save="changeUserPhone"
  />
  <ModalBox
    v-if="showEditpassword"
    :show="showEditpassword"
    title="Изменение пароля"
    ok="Сохранить"
    @ok="changeUserPassword"
    @cancel="showEditpassword = false"
  >
    <div class="flex flex-col w-full">
      <div>
        <p class="mb-[10px] mt-[10px]">
          Введите старый пароль
        </p>
        <input
          v-model="oldPassword"
          type="text"
          style="-webkit-text-security: disc;"
          autocomplete=""
          class="bg-[#f4f5f7]/50 rounded-[6px] border border-[#4c4c4d] focus:border-[#ff9123] w-full px-[14px] py-[11px] text-[14px] leading-[16px] text-[#4c4c4d] font-roboto"
        >
      </div>
      <p
        v-if="invalidOldPassword === 'old_password invalid'"
        class="text-red-500 text-xs mt-1 ml-1"
      >
        Корректно введите старый пароль
      </p>
      <p
        v-if="emptyOldPass"
        class="text-red-500 text-xs mt-1 ml-1"
      >
        Введите старый пароль
      </p>
      <div>
        <p class="mb-[10px] mt-[10px]">
          Введите новый пароль
        </p>
        <input
          v-model="newPassword"
          type="text"
          style="-webkit-text-security: disc;"
          autocomplete=""
          class="bg-[#f4f5f7]/50 rounded-[6px] border border-[#4c4c4d] focus:border-[#ff9123] w-full px-[14px] py-[11px] text-[14px] leading-[16px] text-[#4c4c4d] font-roboto"
        >
      </div>
      <div>
        <p class="mb-[10px] mt-[10px]">
          Подтвердите новый пароль
        </p>
        <input
          v-model="confirmNewPassword"
          type="text"
          style="-webkit-text-security: disc;"
          autocomplete=""
          class="bg-[#f4f5f7]/50 rounded-[6px] border border-[#4c4c4d] focus:border-[#ff9123] w-full px-[14px] py-[11px] text-[14px] leading-[16px] text-[#4c4c4d] font-roboto"
        >
      </div>
      <p
        v-if="emptyNewPasses"
        class="text-red-500 text-xs mt-1 ml-1"
      >
        Заполните все поля ввода нового пароля
      </p>
      <p
        v-if="newPassError"
        class="text-red-500 text-xs mt-1 ml-1"
      >
        Пароли не совпадают
      </p>
    </div>
  </ModalBox>
  <form class="mx-6 overscroll-auto bg-white p-2 rounded">
    <div class="flex pb-3">
      <form class="text-left w-40">
        <div class="text-center mb-3 mr-5">
          <div class="pr-2">
            <span class="circle-image">
              <img
                v-if="$store.state.user.user?.foto_link"
                :src="$store.state.user.user?.foto_link"
                class="rounded-[27px] content-center object-cover"
              >
            </span>
          </div>
          <div>
            <input
              id="iconfile"
              type="file"
              class="hidden"
              accept="image/png, image/jpeg"
              @change="changeUserPhoto"
            >
            <label
              for="iconfile"
              class="text-[13px] mr-3 justify-center cursor-pointer"
            >
              изменить фото
            </label>
            <br>
          </div>
        </div>
      </form>
      <form class="text-left w-64">
        <div class="font-medium text-[roboto] mb-4 text-base landing-[19px]">
          Тип аккаунта
        </div>
        <p
          class="text-sm font-medium landing-4"
        >
          {{ tarifText }}
        </p>
        <p
          v-if="$store.state.user.user?.date_expired"
          class="mt-1 text-sm font-normal font-[Roboto] landing-5 text-[#606061]"
        >
          <a>{{ $store.state.user.user?.date_expired }}({{ $store.state.user.user?.days_left ?? 0 }})</a>
        </p>
        <div class="mt-2">
          <button
            class="font-normal text-sm landing-4"
            type="button"
            @click="changeCurrentTab ('tarif')"
          >
            <p class="border border-gray-400 rounded-md p-2.5 text-gray-600 mt-2">
              Управление тарифом
            </p>
          </button>
        </div>
        <div class="mt-6">
          <p class="text-base font-medium mb-2 text-[#4C4C4D]">
            Имя
          </p>
          <form class="mb-2">
            <div class="text-sm landing-4 font-normal">
              {{ $store.state.user.user?.current_user_name ?? '' }}
            </div>
            <button
              type="button"
              class="mt-2 text-[13px] landing-[13px] text-[#007BE5]"
              @click="showEditname = true"
            >
              Изменить имя
            </button>
          </form>
          <div class="mt-6">
            <p class="text-base font-medium mb-2 text-[#4C4C4D]">
              Телефон
            </p>
            <form class="mb-2">
              <div class="text-sm landing-4 font-normal">
                {{ userPhone() }}
              </div>
              <button
                type="button"
                class="mt-2 text-[13px] landing-[13px] text-[#007BE5]"
                @click="showEditphone = true"
              >
                Изменить телефон
              </button>
            </form>
          </div>
          <div class="mb-2 mt-6">
            <p class="text-base font-medium mb-2 text-[#4C4C4D]">
              Email
            </p>
            <div
              contenteditable="false"
              class="text-[13px] landing-[13px]"
            >
              {{ $store.state.user.user?.current_user_email }}
            </div>
          </div>
          <div class="mb-2 mt-6">
            <form>
              <p class="text-base font-medium mb-2 text-[#4C4C4D]">
                Пароль
              </p>
              <button
                type="button"
                class="mt-2 text-[13px] landing-[13px] text-[#007BE5]"
                @click="showPasswordModalBox"
              >
                Изменить пароль
              </button>
            </form>
          </div>
          <div class="mb-2 mt-6">
            <button
              type="button"
              class="mb-2 bg-[#d9d9d9] text-black p-2 rounded-md"
              @click="startOnBoarding"
            >
              Режим тестирования
            </button>
            <form>
              <button
                class="bg-orange-400 text-white mt-2 text-base p-2 rounded-md"
                @click="logout()"
              >
                Выйти из аккаунта
              </button>
            </form>
          </div>
        </div>
      </form>
    </div>
  </form>
</template>

<script>
import { USER_CHANGE_PHOTO, USER_CHANGE_PHONE } from '@/store/actions/user.js'
import { AUTH_CHANGE_PASSWORD } from '@/store/actions/auth.js'
import { CHANGE_EMPLOYEE_NAME } from '@/store/actions/employees.js'
import { USER_START_ONBOARDING } from '@/store/actions/onboarding.js'
import BoardModalBoxRename from '@/components/Board/BoardModalBoxRename.vue'
import ModalBox from '@/components/modals/ModalBox.vue'

export default {
  components: {
    ModalBox,
    BoardModalBoxRename
  },
  emits: ['AccLogout', 'currentSettingsTab'],
  data () {
    return {
      oldPassword: '',
      newPassword: '',
      confirmNewPassword: '',
      invalidOldPassword: '',
      showEditname: false,
      newPassError: false,
      emptyOldPass: false,
      emptyNewPasses: false,
      showEditphone: false,
      showEditpassword: false
    }
  },
  computed: {
    tarifText () {
      switch (this.$store.state.user.user?.tarif) {
        case 'trial':
          return 'Пробная версия'
        case 'free':
          return 'Закончилась лицензия, Истек триал, Превышен лимит рабочих мест'
        case 'expert':
          return 'Действительная лицензия с одним рабочим местом'
        case 'business':
          return 'Действительная лицензия с несколькими рабочими местами'
        default:
          return this.$store.state.user.user?.tarif
      }
    }
  },
  methods: {
    changeCurrentTab (tabName) {
      this.$emit('currentSettingsTab', tabName)
    },
    logout () {
      this.$emit('AccLogout')
    },
    startOnBoarding () {
      this.$store.dispatch(USER_START_ONBOARDING)
      this.$router.push('/doitnow')
    },
    changeUserPhoto (event) {
      const files = event.target.files
      const formData = new FormData()
      const file = files[0]
      formData.append('files[0]', file)
      const data = {
        file: formData
      }
      this.$store.dispatch(USER_CHANGE_PHOTO, data)
    },
    showPasswordModalBox () {
      this.emptyNewPasses = false
      this.emptyOldPass = false
      this.invalidOldPassword = false
      this.newPassError = false

      this.showEditpassword = true
    },
    changeUserPassword () {
      const oldPassword = this.oldPassword
      const newPassword = this.newPassword
      const isFieldNotEmptyAndRight = oldPassword && this.newPassword && this.confirmNewPassword && this.newPassword === this.confirmNewPassword
      const data = {
        old_password: oldPassword,
        new_password: newPassword
      }
      if (isFieldNotEmptyAndRight) {
        this.$store.dispatch(AUTH_CHANGE_PASSWORD, data).then(() => {
          this.showEditpassword = false
          this.oldPassword = ''
          this.newPassword = ''
          this.confirmNewPassword = ''
        })

        this.$store.dispatch(AUTH_CHANGE_PASSWORD, data).catch(err => {
          this.invalidOldPassword = err.error
          this.oldPassword = ''
          this.confirmNewPassword = ''
          this.showEditpassword = true
          this.emptyOldPass = false
        })
      } else if (this.newPassword !== this.confirmNewPassword) {
        this.newPassError = true
      }
      // проверяем пустое поле старого пароля
      oldPassword ? this.emptyOldPass = false : this.emptyOldPass = true
      // проверяем пустые поля нового пароля
      this.newPassword && this.confirmNewPassword ? this.emptyNewPasses = false : this.emptyNewPasses = true
    },

    changeUserPhone (phone) {
      this.showEditphone = false
      const date = new Date()
      const timezone = date.getTimezoneOffset() / 60 * (-1)
      const data = {
        phone: phone,
        timezone: timezone
      }
      this.$store.dispatch(USER_CHANGE_PHONE, data)
    },

    changeUserName (name) {
      this.showEditname = false
      const data = {
        name: name,
        email: this.$store.state.user.user.current_user_email
      }
      this.$store.dispatch(CHANGE_EMPLOYEE_NAME, data)
    },

    userName () {
      const name = this.$store.state.user.user.current_user_name ?? ''
      return name
    },

    userPhone () {
      const phone = this.$store.state.user.user?.current_user_phone ?? ''
      const index = phone.lastIndexOf(' ("')
      if (index !== -1) {
        return phone.slice(0, index)
      }
      return phone
    }
  }
}
</script>

<style scoped>

.circle-image{
  display: inline-block;
  border-radius: 10px;

}
.circle-image img{
  width: 106px;
  height: 106px;
}
</style>
