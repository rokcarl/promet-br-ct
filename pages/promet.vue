<template>
  <section class='container-page'>
    <div>
      <logo/>
      <h1 class='title-page'>{{ loc['Title'] }}</h1>
      <p>Updated: {{ now.toLocaleString() }}</p>
      <table class='data'>
        <thead>
          <tr>
            <th>Direction</th>
            <th>Speed</th>
            <th>Gap</th>
            <th>Count / hour</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in loc['Data']">
            <td>{{ item['properties']['stevci_smerOpis'] }}</td>
            <td class="num">{{ item['properties']['stevci_hit'] }} km/h</td>
            <td class="num">{{ item['properties']['stevci_gap'] }} s</td>
            <td class="num">{{ item['properties']['stevci_stev'] }}</td>
          </tr>
        </tbody>
      </table>

      <a @click="$router.go({path:'/promet', force: true})" class="button--green">Refresh</a>
    </div>
  </section>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  },
  async asyncData({ app }) {
    const now = new Date
    const locDescription = 'Brežice'
    const countersData = await app.$axios.$get('http://opendata.si/promet/counters/')
    const traffic = countersData['Contents'][0]['Data']['Items']
    const loc = traffic.filter((t) => t['stevci_lokacijaOpis'] == locDescription)[0]
    return { loc, now }
  }
}
</script>

<style>
.title-page {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 50px;
  color: #35495e;
  letter-spacing: 1px;
}
.container-page {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
table.data {
  margin: 20px auto;
}
table.data td, table.data th {
  border: 1px solid #ddd;
  border-collapse: collapse;
  padding: 2px 20px;
}
td.num, th.num {
  text-align: right;
}
</style>
