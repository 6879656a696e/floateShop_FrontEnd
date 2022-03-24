<template>
  <v-row justify="center" class="mt-12 mb-12">
    <v-col
      cols="12"
      sm="10"
      md="8"
      lg="6"
    >
      <v-card ref="form" id="signupForm" class="rounded-lg" elevation="0">
        <v-card-text>
            <div class="inputBoxWrap">
              <span class="formTitle">이름</span>
              <v-text-field
                ref="name"
                v-model="name"
                :rules="[() => !!name || '성함은 필수입력 항목입니다.']"
                :error-messages="errorMessages"
                placeholder="홍길동"
                required
              ></v-text-field>
            </div>


            <div class="inputBoxWrap">
              <span class="formTitle">국가</span>
          <v-autocomplete
            ref="country"
            v-model="country"
            :rules="[() => !!country || '국가는 필수 입력 항목입니다.']"
            :items="countries"
            placeholder="선택"
            required
          ></v-autocomplete>
            </div>


            <div class="inputBoxWrap">
              <span class="formTitle">도</span>
          <v-text-field
            ref="state"
            v-model="state"
            :rules="[() => !!state || '도는 필수 입력 항목입니다.']"
            required
            placeholder="경기도"
          ></v-text-field>
            </div>

            <div class="inputBoxWrap">
              <span class="formTitle">시/군/구</span>
          <v-text-field
            ref="city"
            v-model="city"
            :rules="[() => !!city || '정확한 시/군/구를 입력해주세요.', addressCheck]"
            placeholder="용인시 기흥구"
            required
          ></v-text-field>
            </div>

            <div class="inputBoxWrap">
              <span class="formTitle">상세주소</span>
          <v-text-field
            ref="address"
            v-model="address"
            :rules="[
              () => !!address || '배송시 받으실 주소는 필수 입력 항목입니다.',
              () => !!address && address.length <= 5 || '최소 다섯 글자 이상 입력해주세요.',
              addressCheck
            ]"
            placeholder="OO빌딩 203호"
            counter="25"
            required
          ></v-text-field>
          
            </div>

            <div class="inputBoxWrap">
              <span class="formTitle">우편번호</span>
          <v-text-field
            ref="zip"
            v-model="zip"
            :rules="[() => !!zip || '우편번호는 필수입력 항목입니다.']"
            required
            placeholder="12345"
          ></v-text-field>
            </div>
        </v-card-text>
        <v-card-actions  class="mt-12">
          <v-btn text outlined color="error">
            취소
          </v-btn>
          <v-spacer></v-spacer>
          <v-slide-x-reverse-transition>
            <v-tooltip
              v-if="formHasErrors"
              left
            >
              <template v-slot:activator="{ on, attrs }">
                <v-btn
                  icon
                  class="my-0"
                  v-bind="attrs"
                  @click="resetForm"
                  v-on="on"
                >
                  <v-icon>mdi-refresh</v-icon>
                </v-btn>
              </template>
              <span>Refresh form</span>
            </v-tooltip>
          </v-slide-x-reverse-transition>
          <v-btn
          outlined
            color="success"
            text
            @click="submit"
          >
            회원가입 완료
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
  export default {
    data: () => ({
      countries: ['Afghanistan', 'Albania', 'Algeria', 'Andorra', 'Angola', 'Anguilla', 'Antigua &amp; Barbuda', 'Argentina', 'Armenia', 'Aruba', 'Australia', 'Austria', 'Azerbaijan', 'Bahamas', 'Bahrain', 'Bangladesh', 'Barbados', 'Belarus', 'Belgium', 'Belize', 'Benin', 'Bermuda', 'Bhutan', 'Bolivia', 'Bosnia &amp; Herzegovina', 'Botswana', 'Brazil', 'British Virgin Islands', 'Brunei', 'Bulgaria', 'Burkina Faso', 'Burundi', 'Cambodia', 'Cameroon', 'Cape Verde', 'Cayman Islands', 'Chad', 'Chile', 'China', 'Colombia', 'Congo', 'Cook Islands', 'Costa Rica', 'Cote D Ivoire', 'Croatia', 'Cruise Ship', 'Cuba', 'Cyprus', 'Czech Republic', 'Denmark', 'Djibouti', 'Dominica', 'Dominican Republic', 'Ecuador', 'Egypt', 'El Salvador', 'Equatorial Guinea', 'Estonia', 'Ethiopia', 'Falkland Islands', 'Faroe Islands', 'Fiji', 'Finland', 'France', 'French Polynesia', 'French West Indies', 'Gabon', 'Gambia', 'Georgia', 'Germany', 'Ghana', 'Gibraltar', 'Greece', 'Greenland', 'Grenada', 'Guam', 'Guatemala', 'Guernsey', 'Guinea', 'Guinea Bissau', 'Guyana', 'Haiti', 'Honduras', 'Hong Kong', 'Hungary', 'Iceland', 'India', 'Indonesia', 'Iran', 'Iraq', 'Ireland', 'Isle of Man', 'Israel', 'Italy', 'Jamaica', 'Japan', 'Jersey', 'Jordan', 'Kazakhstan', 'Kenya', 'Kuwait', 'Kyrgyz Republic', 'Laos', 'Latvia', 'Lebanon', 'Lesotho', 'Liberia', 'Libya', 'Liechtenstein', 'Lithuania', 'Luxembourg', 'Macau', 'Macedonia', 'Madagascar', 'Malawi', 'Malaysia', 'Maldives', 'Mali', 'Malta', 'Mauritania', 'Mauritius', 'Mexico', 'Moldova', 'Monaco', 'Mongolia', 'Montenegro', 'Montserrat', 'Morocco', 'Mozambique', 'Namibia', 'Nepal', 'Netherlands', 'Netherlands Antilles', 'New Caledonia', 'New Zealand', 'Nicaragua', 'Niger', 'Nigeria', 'Norway', 'Oman', 'Pakistan', 'Palestine', 'Panama', 'Papua New Guinea', 'Paraguay', 'Peru', 'Philippines', 'Poland', 'Portugal', 'Puerto Rico', 'Qatar', 'Reunion', 'Romania', 'Russia', 'Rwanda', 'Saint Pierre &amp; Miquelon', 'Samoa', 'San Marino', 'Satellite', 'Saudi Arabia', 'Senegal', 'Serbia', 'Seychelles', 'Sierra Leone', 'Singapore', 'Slovakia', 'Slovenia', 'South Africa', 'South Korea', 'Spain', 'Sri Lanka', 'St Kitts &amp; Nevis', 'St Lucia', 'St Vincent', 'St. Lucia', 'Sudan', 'Suriname', 'Swaziland', 'Sweden', 'Switzerland', 'Syria', 'Taiwan', 'Tajikistan', 'Tanzania', 'Thailand', `Timor L'Este`, 'Togo', 'Tonga', 'Trinidad &amp; Tobago', 'Tunisia', 'Turkey', 'Turkmenistan', 'Turks &amp; Caicos', 'Uganda', 'Ukraine', 'United Arab Emirates', 'United Kingdom', 'United States', 'Uruguay', 'Uzbekistan', 'Venezuela', 'Vietnam', 'Virgin Islands (US)', 'Yemen', 'Zambia', 'Zimbabwe'],
      errorMessages: '',
      name: null,
      address: null,
      city: null,
      state: null,
      zip: null,
      country: 'South Korea',
      formHasErrors: false,
    }),

    computed: {
      form () {
        return {
          name: this.name,
          address: this.address,
          city: this.city,
          state: this.state,
          zip: this.zip,
          country: this.country,
        }
      },
    },

    watch: {
      name () {
        this.errorMessages = ''
      },
    },

    methods: {
      addressCheck () {
        this.errorMessages = this.address && !this.name
          ? `정확한 주소를 입력해주세요.`
          : ''

        return true
      },
      resetForm () {
        this.errorMessages = []
        this.formHasErrors = false

        Object.keys(this.form).forEach(f => {
          this.$refs[f].reset()
        })
      },
      submit () {
        this.formHasErrors = false

        Object.keys(this.form).forEach(f => {
          if (!this.form[f]) this.formHasErrors = true

          this.$refs[f].validate(true)
        })
      },
    },
  }
</script>

<style scoped>
formTitle {
        display: block;
        margin: 20px 0;
    }

    .inputBoxWrap {
        margin: 50px 0;
    }
</style>