## Java

- Algorithms
	- [SET_01](Java/algo/SET_Algo_01.md)
	- [SET_02](Java/algo/SET_Algo_02.md)
	- [SET_03](Java/algo/SET_Algo_03.md)
- [Hibernate](Java/LC_Hibernate.md)
- [Core](Java/LC_Java_Core.md)
- [Regex](Java/LC_Java_Regex.md)
- [Write something](Java/LC_Java_Write_Something.md)
- [Stream API](QA/Livecoding/Java/LC_Stream_API.md) ![](https://img.shields.io/badge/Stream_API_задачек-${LC_STREAM_API_COUNT}-blue)

<script>
  fetch('https://github.com/GitLobanov/interview-java-backend/actions/runs/')
    .then(r => r.json())
    .then(data => {
      document.getElementById('badge').src = 
        `https://img.shields.io/badge задачек-${data.workflow_runs[0].LC_STREAM_API_COUNT}-blue`;
    });
</script>

<img id="badge">

## Rest

- [URL](REST/LC_REST_URL.md)

## SQL

- [SQL](SQL/LC_SQL.md)