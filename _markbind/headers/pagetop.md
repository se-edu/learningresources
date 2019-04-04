<header>
<navbar placement="top" type="light">
<a slot="brand" href="https://se-edu.github.io" title="More SE-EDU Resources" class="navbar-brand"><img src="https://se-edu.github.io/images/SeEduLogo.png" alt="SE-EDU" width="30"></a>
  <li><a href="https://se-edu.github.io/addressbook-level1/" class="nav-link"><md>AB-1</md></a></li>
  <li><a href="https://se-edu.github.io/addressbook-level2/" class="nav-link"><md>AB-2</md></a></li>
  <li><a href="https://se-edu.github.io/addressbook-level3/" class="nav-link"><md>AB-3</md></a></li>
  <li><a href="https://se-edu.github.io/addressbook-level4/" class="nav-link"><md>AB-4</md></a></li>
  <li><a href="https://se-edu.github.io/collate/" class="nav-link"><md>Collate</md></a></li>
  <li><a href="https://se-edu.github.io/se-book/" class="nav-link"><md>Book</md></a></li>
  <dropdown text="Resources" class="nav-link">
    <li><a href="{{baseUrl}}/index.html" class="dropdown-item"><md>Home</md></a></li>
    <li><a href="{{baseUrl}}/contributing.html" class="dropdown-item"><md>Contribute</md></a></li>
    <li><a href="{{baseUrl}}/about.html" class="dropdown-item"><md>About</md></a></li>
    <li><a href="https://github.com/se-edu/learningresources" class="dropdown-item"><md>{{ fab_github }} GitHub</md></a></li>
  </dropdown>
  <li slot="right" class="nav-link">
    <form class="navbar-form">
      <searchbar :data="searchData" placeholder="Search Learning Resources" :on-hit="searchCallback" menu-align-right ></searchbar>
    </form>
  </li>
</navbar>
</header>