<template>
  <div>
    <table
      cellspacing="0"
      cellpadding="0"
      border="0"
      role="presentation"
      style="font-family: Arial, Helvetica, sans-serif; line-height: 1.5"
      :style="{
        fontSize: options.font.size + 'px',
        fontFamily: options.font.family
      }"
    >
      <tbody>
        <tr>
          <table
            cellspacing="0"
            cellpadding="0"
            border="0"
            role="presentation"
            :style="{fontSize: options.font.size + 'px'}"
          >
            <tbody>
              <!-- Name field -->
              <tr>
                <td>
                  <span
                    :style="{color: options.color.mainPreview || options.color.main, fontWeight: 600, fontSize: `${options.font.size + 2}px`}"
                  >{{ mainFields[0].value }}</span>
                </td>
              </tr>
              <!-- Job title field -->
              <tr>
                <td>
                  <span>{{ mainFields[1].value }}</span>
                </td>
              </tr>
              <!-- Avatar field -->
              <tr>
                <td style="padding-top: 8px;">
                  <avatar
                    :show-avatar="showAvatar"
                    :src="image"
                    :size="options.avatar.size"
                    :roundness="options.avatar.roundness"
                  />
                </td>
              </tr>
              <!-- Company field -->
              <tr>
                <td
                  style="font-weight: bold;"
                  :style="{paddingTop: showAvatar ? '8px' : '0px'}"
                >{{ mainFields[2].value }}</td>
              </tr>
              <!-- Email field -->
              <tr>
                <td style="padding-top: 8px; font-weight: 600;">
                  <span
                    v-if="otherFields[1].value"
                    :style="{color: options.color.mainPreview || options.color.main}"
                  >
                    e.
                    <span>
                      <a
                        style="text-decoration: none; color: black; font-weight: normal;"
                        :href="`mailto:${otherFields[1].value}`"
                      >{{ otherFields[1].value }}</a>
                    </span>
                  </span>
                </td>
              </tr>
              <!-- Website field -->
              <tr>
                <td style="font-weight: 600;">
                  <span
                    v-if="otherFields[0].value"
                    :style="{color: options.color.mainPreview || options.color.main}"
                  >
                    w.
                    <span>
                      <a
                        style="text-decoration: none; color: black; font-weight: normal;"
                        :href="formatLink(otherFields[0].value)"
                      >{{ formatLink(otherFields[0].value) }}</a>
                    </span>
                  </span>
                </td>
              </tr>
              <!-- Phone number field -->
              <tr>
                <td style="font-weight: 600;">
                  <span
                    v-if="otherFields[2].value"
                    :style="{color: options.color.mainPreview || options.color.main}"
                  >
                    t.
                    <span style="color: black; font-weight: normal;">{{ otherFields[2].value }}</span>
                  </span>
                </td>
              </tr>
              <!-- Other fields -->
              <template v-for="(item, index) in otherFields">
                <tr
                  v-if="item.value && index > 2"
                  :key="item.name"
                >
                  <td>
                    <a
                      v-if="item.type === 'link'"
                      :href="formatLink(item.value)"
                      style="text-decoration: none; color: inherit;"
                    >{{ item.value }}</a>
                    <a
                      v-if="item.type === 'email'"
                      :href="`mailto:${item.value}`"
                      style="text-decoration: none; color: inherit;"
                    >{{ item.value }}</a>
                    <span v-if="item.type === 'text'">{{ item.value }}</span>
                  </td>
                </tr>
              </template>
              <!-- Social icons -->
              <tr v-if="socials.installed.length">
                <td>
                  <table
                    cellspacing="2"
                    cellpadding="0"
                    border="0"
                    role="presentation"
                    style="margin-top: 16px; margin-left: -2px;"
                  >
                    <tbody>
                      <tr>
                        <td
                          v-for="item in socials.installed"
                          :key="item.name"
                          align="center"
                          style="width: 20px; height: 20px; border-radius: 3px;"
                          :style="{
                            backgroundColor: options.color.mainPreview || options.color.main,
                          }"
                        >
                          <a :href="formatLink(item.link)">
                            <img
                              width="12px"
                              :src="`${s3url}/icons/${item.icon}.png`"
                              :alt="`social-icon-${item.icon}`"
                              style="display: table-cell; vertical-align: middle;"
                            >
                          </a>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </td>
              </tr>
            </tbody>
          </table>
        </tr>
      </tbody>
    </table>
    <!-- Addons -->
    <table
      v-if="isAdded('mobileApp')"
      cellspacing="0"
      cellpadding="0"
      border="0"
      role="presentation"
      style="margin-top: 10px;"
    >
      <tbody>
        <tr>
          <td v-if="addons.mobileApp.appStore.link">
            <a :href="addons.mobileApp.appStore.link">
              <img
                :src="addons.mobileApp.appStore.img"
                style="height:35px; margin-right: 5px;"
                alt="app store badge"
              >
            </a>
          </td>
          <td v-if="addons.mobileApp.googlePlay.link">
            <a :href="addons.mobileApp.googlePlay.link">
              <img
                :src="addons.mobileApp.googlePlay.img"
                style="height:35px;"
                alt="google play badge"
              >
            </a>
          </td>
        </tr>
      </tbody>
    </table>
    <table
      v-if="isAdded('disclaimer')"
      role="presentation"
      :style="{
        fontSize: options.font.size + 'px',
        fontFamily: options.font.family,
        color: '#888',
        marginTop: '10px'
      }"
    >
      <tbody>
        <tr>
          <td>
            <span>{{ addons.disclaimer }}</span>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import EmailTemplate from './emailTemplate'
import Avatar from './components/Avatar'

export default {
  components: { Avatar },
  extends: EmailTemplate
}
</script>
