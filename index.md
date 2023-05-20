---
layout: default
navn: ledighedsdato.dk
---
{::nomarkdown}
<div id="app" class="container">
    <a href="/"><h3 class="title has-text-centered">ledighedsdato.dk</h3></a>

    <div class="filter-container container">
      <label>Filtrer efter kompetencer:</label>
      <div class="checkbox-group">
        <div v-for="kompetence in kompetencer" :key="kompetence">
          <input type="checkbox" :value="kompetence" v-model="selectedKompetencer" @change="updateURL" :id="'checkbox-' + kompetence" class="checkbox-input">
          <label :for="'checkbox-' + kompetence" class="checkbox-button tag">${ kompetence }</label>
        </div>
      </div>
    </div>

  <div class="container">
    <ul class="konsulent-list">
      <div v-if="filteredKonsulenter.length === 0">
          <p>Ingen konsulenter matchede dine valgte kriterier.</p>
      </div>
    <li v-for="konsulent in filteredKonsulenter" class="konsulent">
        <a :href="konsulent.link" class="navn">${ konsulent.navn }</a>
        <span class="dato">Ledig fra: ${ konsulent.ledighedsdato }</span>
      </li>
    </ul>
  </div>
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
        for(k of this.selectedKompetencer)
        {
          params.append('kompetencer', k);
        }
        history.replaceState({}, '', '?'+ params.toString());
      },
      loadFilterFromURL: function() {
        var params = new URLSearchParams(window.location.search);
        var kompetencer = params.getAll('kompetencer');
        this.selectedKompetencer = kompetencer;
        console.log(this.selectedKompetencer);
      }
    },
    created: function() {
      var self = this;
      self.loadFilterFromURL();
      this.konsulenter.forEach(function(konsulent) {
         konsulent.kompetencer.forEach(function(kompetence) {
          if (!self.kompetencer.includes(kompetence)) {
            self.kompetencer.push(kompetence);
          }
         })
       })
    }
});
</script>
