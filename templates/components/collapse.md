{% component collapse(title:string) %}
<details>
    <summary> {{title | safe }} </summary>

{{ body }}

</details>
{% endcomponent collapse %}
