## 개요
- jsp => React migration 과정에서의 기본 세팅
- react-router-dom v5,v6
  - [공식](https://reactrouter.com/en/main)   
- PrivateRoute
- source
  - index.js
  - App.js
  - RouterPage.js
  - MainLayout.js
  - Sidbar.js, routes.js
## react-router-dom
## PrivateRoute
## source
- index.js
  -  react-router-dom에서 router를 사용하기위해 App을 BrowserRouter로 감싸준다   
```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import { BrowserRouter} from "react-router-dom";

const root = ReactDOM.createRoot(document.getElementById("root"));

root.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals

//reportWebVitals(console.log) - CRA로 생성한 프로젝트일때 앱 퍼포먼스 시간 분석을 객체형태로 출력
```
- App.js
```jsx
import React from "react";
import RouterPage from "RouterPage";

function App() {
    
    return (
        <>    
            <RouterPage/>          
        </>
    );
}

export default App;
```
- RouterPage.js
  - PrivateRoute 설정으로 user or 인증 여부에 따라 접근권한 제한
  - LoginPage로, 있으면 MainLayout으로 접근가능하도록    
```jsx
import React from "react";
import { Route, Routes, Navigate} from "react-router-dom";
import MainLayout from "layouts/MainLayout";
// ...

const RouterPage = () => {

  //PrivateRoute 에서 체크할 임시 user값
  const user = "tester"
  
  //로그인 여부에 따라 children으로 받은 라우터에 선택적으로 접근가능하도록 보호
  const PrivateRoute = ({children}) =>{
    if(!user){  
      return <Navigate to="/login" />;
    }
    return children;
  }

    return (
       <>      
        <Routes>         
          <Route path="/login" element={<LoginPage />} />
          
          <Route path="/" element={<PrivateRoute><MainLayout /></PrivateRoute>}>
            <Route index element={<Navigate to="dashboard" />} />
            <Route path="dashboard" element={<Dashboard />} />
            <Route path="user" element={<UserProfile />} />
            <Route path="table" element={<TableList />} />
            <Route path="typography" element={<Typography />} />
            <Route path="icons" element={<Icons />} />
            <Route path="maps" element={<Maps />} />
            <Route path="notifications" element={<Notifications />} />
            
            <Route path="unitTest/results" element={<TestResults />} />
            <Route path="regressionTest/results" element={<RegressionTestResults />} />
            <Route path="regressionTest/results/details/:param" element={<RegressionTestDetails />} />
          </Route>
        </Routes>                        
       </>
    );
  };  
  export default RouterPage;
```
- MainLayout.js
  - 중첩라우팅
  - routes에서 sidebarPath를 props로 받고있다. (페에지에서 이동하는 path정보가 저장되어 있다.)
```jsx
import React from "react";
import Header from "./AdminNavbar";
import Sidebar from "./Sidebar";
import Footer from "./Footer";
import SidebarPath from "../routes"
import { Outlet } from "react-router";

function MainLayout() {
    return (
        <>                   
            <Sidebar SidebarPath={SidebarPath} />
            <div className="main-panel">
                <Header/>
                <div className="content">
                    <Outlet />
                </div>
                <Footer/>
            </div>           
        </>
    );
}
export default MainLayout;
```
- routes.js, Sidebar.js
```jsx
/*!
====================================================================
*  사이드메뉴에서 사용하는 routes 정보 파일
====================================================================

* react-router-dom v6에선(정적라우팅) 컴포넌트를 코드에서 
* 직접 정의하고 element prop으로 전달하기를 권장
* 이를 통해 RouterPage에서 전체 컴포넌트구성을 한눈에 파악할 수 있음 
*  
* 위의 내용이 있어 사이드바는 DRY'd 작성하기위한 config 파일

* (기존메뉴 활용 중 추후 삭제 예정)
* (auth관련 메뉴렌더링 부분 어떻게 할지 고민해봐야함)

====================================================================
*/

const SidebarPath = {
  "기존메뉴":[
    { path: "dashboard", submenu:"대시보드" }, 
    { path: "user", submenu:"사용자"},
    { path: "table", submenu:"테이블"},
    { path: "typography", submenu:"글꼴"},
    { path: "icons", submenu:"아이콘"},
    { path: "maps", submenu:"지도"},
    { path: "notifications", submenu:"알림"}],
  "단위테스트":[{ path: "unitTest/results", submenu:"테스트결과"}],
  "회귀테스트":[{ path: "regressionTest/results", submenu:"테스트결과"}]};

export default SidebarPath;


/*!
=========================================================
* 권한 별 메뉴 display 설정 필요
=========================================================

* 상위컴포넌트에서 할 예정

=========================================================
*/

import React, { useState } from "react";
import { useLocation, NavLink } from "react-router-dom";
import { Collapse, Nav } from "react-bootstrap";

function Sidebar({ SidebarPath }) {
  const location = useLocation();
  
  //하위 메뉴 액티브
  const activeRoute = (routeName) => {
    return location.pathname.indexOf(routeName) > -1 ? "active" : "";
  };
  //메뉴리스트
  const menus = Object.keys(SidebarPath);
  
  // 현재 열린 메뉴의 이름을 추적
  const [openMenu, setOpenMenu] = useState(""); 

  //메뉴 토글
  const toggleMenu = (menuName) => {
    setOpenMenu((prevMenu) => (prevMenu == menuName ? "" : menuName));
  };
  // 사용자 정보 전역 상태관리 전 예시
  // const { isSysop, userRole, menuAuthorities, systemMenuAuthorities } = getUserPermissions(session);
  
  return (
    <div className="sidebar" >   
      <div className="sidebar-wrapper">
        <div className="logo d-flex align-items-center justify-content-start">
          <a
            href="/"
            className="simple-text logo-mini mx-1"
          >
            <div className="logo-img">
            <img src={`${process.env.PUBLIC_URL}/img/logo_MessageTester_text.png`} alt="..." />  
            </div>
          </a>        
        </div>
        <Nav>
        {menus.map((menu) => (
            <li key={menu}>
              <a onClick={() => toggleMenu(menu)} className="nav-link">
                {menu} 
                <b className={`caret ${openMenu === menu ? "caret-open" : "caret-closed"}`}></b>
              </a>   
              <Collapse in={openMenu === menu}>
                <div className="sub-list"> 
                  {SidebarPath[menu].map((item, index) => (
                    <li key={index} className={activeRoute(item.path)}>  
                      <NavLink to={item.path} className={({ isActive }) => isActive ? "nav-link active" : "nav-link"}>
                        {item.submenu}
                      </NavLink>
                    </li>
                  ))}
                </div>
              </Collapse>
            </li>
        ))}
      </Nav>
      </div>
    </div>
  );
}

export default Sidebar;
```
