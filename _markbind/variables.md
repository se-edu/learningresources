<span id="navbar">
<navbar placement="top">
<a slot="brand" href="https://se-edu.github.io" title="More SE-EDU Resources" class="navbar-brand"><button type="button" class="btn btn-outline-dark"><md>More <img src="https://se-edu.github.io/images/SeEduLogo.png" alt="SE-EDU" width="30"> Projects ...</md></button></a>
  <li><a href="{{baseUrl}}/index.html" class="nav-link"><md>**Home**</md></a></li>
  <li><a href="{{baseUrl}}/contributing.html" class="nav-link"><md>**Contribute**</md></a></li>
  <li><a href="{{baseUrl}}/about.html" class="nav-link"><md>**About**</md></a></li>
  <li><a href="https://github.com/se-edu/learningresources" class="nav-link"><md>{{ fab_github }}</md></a></li>
  <li slot="right" class="nav-link">
    <form class="navbar-form">
      <searchbar :data="searchData" placeholder="Search" :on-hit="searchCallback" menu-align-right ></searchbar>
    </form>
  </li>
</navbar>
</span>
