---
layout: default
---

<h1>Konsulenter</h1>

<div id="kompetence-filter">
  <label for="kompetence-select">Filtrér på kompetence(r):</label>
  <select id="kompetence-select" multiple>
    <option value="">Alle</option>
    {% assign kompetencer = "" %}
    {% for konsulent in site.konsulenter %}
      {% assign konsulent_kompetencer = konsulent.kompetencer | split: "," %}
      {% assign kompetencer = kompetencer | concat: konsulent_kompetencer | uniq | sort %}
    {% endfor %}
    {% for kompetence in kompetencer %}
      <option value="{{ kompetence | escape }}">{{ kompetence }}</option>
    {% endfor %}
  </select>
</div>

{% for konsulent in site.konsulenter %}
<div class="konsulent" {% if konsulent.kompetencer %}data-kompetencer="{{ konsulent.kompetencer | replace: " ", "" | escape }}"{% endif %}>
  <a class="navn" href="{{ konsulent.link }}">{{ konsulent.navn }}</a>
  <span class="dato">{{ konsulent.ledig }}</span>
  {% if konsulent.kompetencer %}
  <ul class="kompetencer">
    {% for kompetence in konsulent.kompetencer | split: "," %}
    <li>{{ kompetence }}</li>
    {% endfor %}
  </ul>
  {% endif %}
</div>
{% endfor %}

<script>
function filtrerKonsulenter() {
  var kompetenceSelect = document.getElementById("kompetence-select");
  var valgteKompetencer = Array.from(kompetenceSelect.selectedOptions).map(option => option.value);

  var konsulenter = document.getElementsByClassName("konsulent");

  for (var i = 0; i < konsulenter.length; i++) {
    var konsulent = konsulenter[i];
    var kompetencer = konsulent.getAttribute("data-kompetencer").split(",").map(kompetence => kompetence.trim());
    var matcherKompetencer = true;

    for (var j = 0; j < valgteKompetencer.length; j++) {
      var valgtKompetence = valgteKompetencer[j];
      if (valgtKompetence !== "" && !kompetencer.includes(valgtKompetence)) {
        matcherKompetencer = false;
        break;
      }
    }

    if (matcherKompetencer) {
      konsulent.style.display = "";
    } else {
      konsulent.style.display = "none";
    }
  }
}

document.getElementById("kompetence-select").addEventListener("change", filtrerKonsulenter);

</script>
