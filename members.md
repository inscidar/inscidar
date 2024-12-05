---
layout: home
title: Members 
permalink: /members/
---

## Members

<table class='team'>
    <tr>
        <th>Name</th>
        <th>Email</th>
        <th>Organization</th>
    </tr>
    {% assign members = site.data.members | sort: 'lastname' %}
    {% for member in members %}
    <tr>
        {% if member.url %}
            <td><a href="{{ member.url }}">{{ member.firstname }} {{ member.middlename }} {{ member.lastname }}</a></td>
        {% else %}
            <td>{{ member.firstname }} {{ member.middlename }} {{ member.lastname }}</td>
        {% endif %}
        <td>{{ member.email }}</td>
        <td>{{ member.organization }}</td>
    </tr>
    {% endfor %}
</table>
