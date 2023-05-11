---
layout: default
---
{::nomarkdown}
<div id="app">
  <h1>Konsulenter</h1>
  <!-- <div>
    <label for="kompetence-select">Filtrer efter kompetence:</label>
    <select id="kompetence-select" v-model="selectedKompetencer" multiple @change="updateURL">
      <option value="">Alle</option>
      <option v-for="kompetence in kompetencer" :value="kompetence">${ kompetence }</option>
    </select>
  </div> -->
  <div>
  <label>Filtrer efter kompetencer:</label>
  <div v-for="kompetence in kompetencer" :key="kompetence">
    <input type="checkbox" :value="kompetence" v-model="selectedKompetencer" @change="updateURL">
    <span>${ kompetence }</span>
  </div>
</div>

  <div v-if="filteredKonsulenter.length === 0">
    <p>Ingen konsulenter matchede dine valgte kriterier.</p>
  </div>
  <ul>
    <li v-for="konsulent in filteredKonsulenter" class="konsulent">
      <a :href="konsulent.link" class="navn">${ konsulent.navn }</a>
      <span class="dato">Ledig fra: ${ konsulent.ledighedsdato }</span>
      <span class="kompetencer">${konsulent.kompetencer.join(' | ')}</span>
    </li>
  </ul>
</div>
{:/nomarkdown}

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
  new Vue({
    delimiters: ['${', '}'],
    el: '#app',
    data: {
      konsulenter: [
        {% for konsulent in site.konsulenter %}
          {
            navn: "{{ konsulent.navn }}",
            ledighedsdato: "{{ konsulent.ledighedsdato }}",
            link: "{{ konsulent.link }}",
            kompetencer: "{{ konsulent.kompetencer }}".split(','),
            linkedin: "{{ konsulent.linkedin }}"
          },
        {% endfor %}
      ],
      kompetencer: [],
      selectedKompetencer: []
    },
    computed: {
        filteredKonsulenter() {
            if (this.selectedKompetencer.length === 0) {
                return this.konsulenter;
            } else {
                return this.konsulenter.filter((konsulent) => {
                    return this.selectedKompetencer.every((kompetence) =>
                    konsulent.kompetencer.includes(kompetence)
                    );
                });
            }
        },
    },
    methods: {
      updateURL: function() {
        var params = new URLSearchParams();
        params.append('kompetencer', this.selectedKompetencer.join(','));
        history.replaceState({}, '', this.selectedKompetencer.length ? '?' + params.toString() : '/');
      },
      loadFilterFromURL: function() {
        var params = new URLSearchParams(window.location.search);
        var kompetencer = params.getAll('kompetencer');
        this.selectedKompetencer = kompetencer;
      }
    },
    created: function() {
      var self = this;
      this.konsulenter.forEach(function(konsulent) {
         console.log(konsulent.navn)
         konsulent.kompetencer.forEach(function(kompetence) {
          if (!self.kompetencer.includes(kompetence)) {
            console.log(" - " + kompetence)
            self.kompetencer.push(kompetence);
          }
         })
       })
    }
});
</script>
