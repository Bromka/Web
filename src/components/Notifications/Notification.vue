<script>
import Employee from '@/components/Notifications/Employee'
import { NAVIGATOR_SUCCESS } from '@/store/actions/navigator'
export default {
  components: {
    Employee
  },
  computed: {
    employees () {
      return this.$store.state.employees.employees
    }
  },
  methods: {

    redirect (notification) {
      const link = `${window.location.origin}/task/${notification.obj.obj.uid}`
      window.location.href = link
      this.$store.commit(NAVIGATOR_SUCCESS)
      localStorage.setItem('lastTab', 'tasks')
    }
  }
}
</script>
<template>
  <NotificationGroup group="top">
    <div class="fixed inset-0 flex items-start justify-end p-6 px-4 py-6 pointer-events-none z-50">
      <div class="w-full max-w-sm">
        <Notification
          v-slot="{ notifications, close }"
          enter="ease-out duration-300 transition"
          enter-from="translate-y-2 opacity-0 sm:translate-y-0 sm:translate-x-4"
          enter-to="translate-y-0 opacity-100 sm:translate-x-0"
          leave="transition ease-in duration-500"
          leave-from="opacity-100"
          leave-to="opacity-0"
          move="transition duration-500"
          move-delay="delay-300"
        >
          <div
            v-for="notification in notifications"
            :key="notification.id"
            class="w-full max-w-sm mt-4 overflow-hidden bg-white rounded-lg shadow-lg pointer-events-auto ring-1 ring-black ring-opacity-5"
          >
            <div class="p-4">
              <div class="flex items-start">
                <div class="shrink-0">
                  <svg
                    class="w-6 h-6 text-green-400"
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                    aria-hidden="true"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"
                    />
                  </svg>
                </div>
                <div
                  class="ml-3 w-0 flex-1 pt-0.5 hover:cursor-pointer"
                  @click="close(notification.id), redirect(notification)"
                >
                  <p class="font-semibold text-gray-800">
                    {{ notification.title }}
                  </p>
                  <Employee
                    v-if="notification.obj && notification.obj.obj && employees[notification.obj.obj.uid_creator]"
                    class="mt-1"
                    :fotolink="employees[notification.obj.obj.uid_creator].fotolink"
                    :name="employees[notification.obj.obj.uid_creator].name"
                  />
                  <Employee
                    v-if="notification.obj && notification.obj.obj && employees[notification.obj.obj.uid_customer]"
                    class="mt-1"
                    :fotolink="employees[notification.obj.obj.uid_customer].fotolink"
                    :name="employees[notification.obj.obj.uid_customer].name"
                  />
                  <p class="text-sm font-semibold text-gray-500">
                    {{ notification.text }}
                  </p>
                </div>
                <div class="flex ml-4 shrink-0">
                  <button
                    class="inline-flex text-gray-400 bg-white rounded-md hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-400"
                    @click="close(notification.id)"
                  >
                    <span class="sr-only">Close</span>
                    <svg
                      class="w-5 h-5"
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 20 20"
                      fill="currentColor"
                      aria-hidden="true"
                    >
                      <path
                        fill-rule="evenodd"
                        d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                        clip-rule="evenodd"
                      />
                    </svg>
                  </button>
                </div>
              </div>
            </div>
          </div>
        </Notification>
      </div>
    </div>
  </NotificationGroup>
</template>
