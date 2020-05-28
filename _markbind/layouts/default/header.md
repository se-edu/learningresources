<header>
<navbar placement="top" type="dark">
<a slot="brand" href="https://se-education.org" title="SE-EDU" class="navbar-brand"><md>:fas-chevron-circle-left: ****SE-EDU****</md></a>
  <li><a href="{{ baseUrl }}/index.html" class="nav-link"><md>**Home**</md></a></li>
  <li><a href="{{ baseUrl }}/about.html" class="nav-link"><md>**About**</md></a></li>
  <li><a href="{{ baseUrl }}/contributing.html" class="nav-link"><md>**Contributing**</md></a></li>
  <li slot="right" class="nav-link">
    <form class="navbar-form">
      <searchbar :data="searchData" placeholder="Search this site" :on-hit="searchCallback" menu-align-right ></searchbar>
    </form>
  </li>
</navbar>
</header>
