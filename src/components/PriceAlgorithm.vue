<template>
  <b-card title="Алгоритм Стоимости Аренды">
    <b-form>
      <div class="mt-3">
        <h5>Базовые настройки</h5>

        <b-form-row>
          <b-form-group
            class="col-md-4"
            label="Минимальная стоимость за месяц"
          >
            <b-form-input
              v-model.number="form.minPrice"
            ></b-form-input>
          </b-form-group>

          <b-form-group
            class="col-md-4"
            label="Количество рабочих дней"
          >
            <b-form-input
              v-model="form.period.days"
            ></b-form-input>
          </b-form-group>

          <b-form-group
            class="col-md-4"
            label="Количество рабочих часов в день"
          >
            <b-form-input
              v-model="form.period.hours"
            ></b-form-input>
          </b-form-group>
        </b-form-row>
      </div>

      <div class="mt-3">
        <h5>Плюс проценты</h5>

        <b-form-row>
          <b-form-group
            class="col-md-6"
            label="+% за если аренда по дням"
            description="Процент, который добавится к стоимости потому что это аренда по дням"
          >
            <b-form-input
              v-model="form.percentByType.days"
            ></b-form-input>
          </b-form-group>

          <b-form-group
            class="col-md-6"
            label="+% за если аренда по часам"
            description="Процент, который добавится к стоимости потому что это аренда по часам"
          >
            <b-form-input
              v-model="form.percentByType.hours"
            ></b-form-input>
          </b-form-group>
        </b-form-row>
      </div>

      <div class="mt-3">
        <h5>Основные настройки</h5>

        <b-form-group class="text-left" label="Район">
          <b-form-radio-group
            v-model="form.district"
            :options="options.district"
            stacked
          ></b-form-radio-group>
        </b-form-group>

        <b-form-group class="text-left" label="Класс">
          <b-form-radio-group
            v-model="form.class"
            :options="options.class"
            stacked
          ></b-form-radio-group>
        </b-form-group>
      </div>
    </b-form>

    <template v-slot:footer>
      <h5>Результаты</h5>
      <b-list-group horizontal class="d-flex align-center justify-content-center">
        <b-list-group-item>Месяц: <b>{{Math.round(result.month)}} тг.</b></b-list-group-item>
        <b-list-group-item>День: <b>{{Math.round(result.day)}} тг.</b></b-list-group-item>
        <b-list-group-item>Час: <b>{{Math.round(result.hour)}} тг.</b></b-list-group-item>
      </b-list-group>
    </template>
  </b-card>
</template>

<script>
export default {
  data() {
    return {
      form: {
        district: 'trsbsk',
        class: 'economy',
        period: {
          days: 30,
          hours: 12,
        },
        minPrice: 100000,
        percentByType: {
          days: 10,
          hours: 20,
        },
      },
      options: {
        district: [
          { text: 'Турсибский', value: 'trsbsk' },
          { text: 'Бостандыкский (+50%)', value: 'bstndksk' },
          { text: 'Медеуский (+100%)', value: 'mdsk' },
        ],
        class: [
          { text: 'Экономный', value: 'economy' },
          { text: 'Стандартный (+25%)', value: 'standard' },
          { text: 'Премиум (+50%)', value: 'premium' },
        ],
      },
    }
  },
  computed: {
    result() {
      let result = {
        month: 0,
        day: 0,
        hour: 0,
      }

      let minPrice = {
        month: this.form.minPrice,
        day: this.form.minPrice / this.form.period.days,
        hour: this.form.minPrice / this.form.period.days / this.form.period.hours,
      }

      switch(this.form.district) {
        case 'trsbsk':
          minPrice.month += 0
          minPrice.day += (minPrice.day * this.form.percentByType.days) / 100
          minPrice.hour += (minPrice.hour * this.form.percentByType.hours) / 100

          switch(this.form.class) {
            case 'economy':
              result.month += minPrice.month
              result.day += minPrice.day
              result.hour += minPrice.hour
              break;
            case 'standard':
              result.month += minPrice.month + (minPrice.month * 25) / 100
              result.day += minPrice.day + (minPrice.day * 25) / 100
              result.hour += minPrice.hour + (minPrice.hour * 25) / 100
              break;
            case 'premium':
              result.month += minPrice.month + (minPrice.month * 50) / 100
              result.day += minPrice.day + (minPrice.day * 50) / 100
              result.hour += minPrice.hour + (minPrice.hour * 50) / 100
              break;
          }
          break;
        case 'bstndksk':
          minPrice.day += ((minPrice.month * 50) / 100) / this.form.period.days
          minPrice.hour += ((minPrice.month * 50) / 100) / this.form.period.days / this.form.period.hours

          minPrice.month += (minPrice.month * 50) / 100
          minPrice.day += (minPrice.day * this.form.percentByType.days) / 100
          minPrice.hour += (minPrice.hour * this.form.percentByType.hours) / 100

          switch(this.form.class) {
            case 'economy':
              result.month += minPrice.month
              result.day += minPrice.day
              result.hour += minPrice.hour
              break;
            case 'standard':
              result.month += minPrice.month + (minPrice.month * 25) / 100
              result.day += minPrice.day + (minPrice.day * 25) / 100
              result.hour += minPrice.hour + (minPrice.hour * 25) / 100
              break;
            case 'premium':
              result.month += minPrice.month + (minPrice.month * 50) / 100
              result.day += minPrice.day + (minPrice.day * 50) / 100
              result.hour += minPrice.hour + (minPrice.hour * 50) / 100
              break;
          }
          break;
        case 'mdsk':
          minPrice.month += (minPrice.month * 100) / 100
          minPrice.day += ((minPrice.month * 50) / 100) / this.form.period.days
          minPrice.hour += ((minPrice.month * 50) / 100) / this.form.period.days / this.form.period.hours

          switch(this.form.class) {
            case 'economy':
              result.month += minPrice.month
              result.day += minPrice.day
              result.hour += minPrice.hour
              break;
            case 'standard':
              result.month += minPrice.month + (minPrice.month * 25) / 100
              result.day += minPrice.day + (minPrice.day * 25) / 100
              result.hour += minPrice.hour + (minPrice.hour * 25) / 100
              break;
            case 'premium':
              result.month += minPrice.month + (minPrice.month * 50) / 100
              result.day += minPrice.day + (minPrice.day * 50) / 100
              result.hour += minPrice.hour + (minPrice.hour * 50) / 100
              break;
          }
          break;
      }

      return result
    }
  },
}
</script>