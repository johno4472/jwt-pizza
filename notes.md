# Learning notes

## JWT Pizza code study and debugging

As part of `Deliverable ⓵ Development deployment: JWT Pizza`, start up the application and debug through the code until you understand how it works. During the learning process fill out the following required pieces of information in order to demonstrate that you have successfully completed the deliverable.

| User activity                                       | Frontend component | Backend endpoints | Database SQL |
| --------------------------------------------------- | ------------------ | ----------------- | ------------ |
| View home page                                      | home.jsx           |  none             | none         |
| Register new user<br/>(t@jwt.com, pw: test)         | register.jsx       | [POST] /api/auth  | `INSERT INTO user (name, email, password) VALUES (?, ?, ?)` <br> `INSERT INTO userRole (userId, role, objectId) VALUES (?, ?, ?)` |
| Login new user<br/>(t@jwt.com, pw: test)            | login.jsx          | [PUT] /api/auth   | `SELECT * FROM user WHERE email=?` <br> `SELECT * FROM userRole WHERE userId=?` <br> `INSERT INTO auth (token, userId) VALUES (?, ?) ON DUPLICATE KEY UPDATE token=token` |
| Order pizza                                         | menu.jsx <br> payment.jsx | [POST] /api/order        | `INSERT INTO dinerOrder (dinerId, franchiseId, storeId, date) VALUES (?, ?, ?, now())` <br> `INSERT INTO orderItem (orderId, menuId, description, price) VALUES (?, ?, ?, ?)` |
| Verify pizza                                        | delivery.jsx  | [POST] /api/order/verify | Not sure (in pizza factory, and code is not public) |
| View profile page                                   | dinerDashboard.jsx | [GET] /api/order  | `SELECT id, franchiseId, storeId, date FROM dinerOrder WHERE dinerId=? LIMIT ${offset},${config.db.listPerPage}` <br> `SELECT id, menuId, description, price FROM orderItem WHERE orderId=?` |
| View franchise<br/>(as diner)                       | frachiseDashboard.jsx | [GET] /api/franchise/${user.id} | `SELECT objectId FROM userRole WHERE role='franchisee' AND userId=?` |
| Logout                                              | logout.jsx  | [DELETE] /api/auth | `DELETE FROM auth WHERE token=?` |
| View About page                                     | about.jsx          | none              | none         |
| View History page                                   | history.jsx        | none              | none         |
| Login as franchisee<br/>(f@jwt.com, pw: franchisee) | login.jsx          | [PUT] /api/auth   | `SELECT * FROM user WHERE email=?` <br> `SELECT * FROM userRole WHERE userId=?` <br> `INSERT INTO auth (token, userId) VALUES (?, ?) ON DUPLICATE KEY UPDATE token=token` |
| View franchise<br/>(as franchisee)                  | frachiseDashboard.jsx | [GET] /api/franchise/${user.id} | `SELECT objectId FROM userRole WHERE role='franchisee' AND userId=?` |
| Create a store                                      | createStore.jsx    | [POST] /api/franchise/${franchise.id}/store | `SELECT u.id, u.name, u.email FROM userRole AS ur JOIN user AS u ON u.id=ur.userId WHERE ur.objectId=? AND ur.role='franchisee'` <br> `SELECT s.id, s.name, COALESCE(SUM(oi.price), 0) AS totalRevenue FROM dinerOrder AS do JOIN orderItem AS oi ON do.id=oi.orderId RIGHT JOIN store AS s ON s.id=do.storeId WHERE s.franchiseId=? GROUP BY s.id` <br> `INSERT INTO store (franchiseId, name) VALUES (?, ?)` |
| Close a store                                       | closeStore.jsx     | [DELETE] /api/franchise/${franchise.id}/store/${store.id} | `SELECT u.id, u.name, u.email FROM userRole AS ur JOIN user AS u ON u.id=ur.userId WHERE ur.objectId=? AND ur.role='franchisee'` <br>`SELECT s.id, s.name, COALESCE(SUM(oi.price), 0) AS totalRevenue FROM dinerOrder AS do JOIN orderItem AS oi ON do.id=oi.orderId RIGHT JOIN store AS s ON s.id=do.storeId WHERE s.franchiseId=? GROUP BY s.id` <br> `DELETE FROM store WHERE franchiseId=? AND id=?` |
| Login as admin<br/>(a@jwt.com, pw: admin)           |login.jsx          | [PUT] /api/auth   | `SELECT * FROM user WHERE email=?` <br> `SELECT * FROM userRole WHERE userId=?` <br> `INSERT INTO auth (token, userId) VALUES (?, ?) ON DUPLICATE KEY UPDATE token=token` |
| View Admin page                                     | adminDashboard.jsx | [GET] /api/franchise?page=${page}&limit=${limit}&name=${nameFilter}` | `SELECT objectId FROM userRole WHERE role='franchisee' AND userId=?` <br> `SELECT id, name FROM franchise WHERE id in (${franchiseIds.join(',')})` |
| Create a franchise for t@jwt.com                    | createFranchise.jsx| [POST] /api/franchise | `SELECT id, name FROM user WHERE email=?` <br> `INSERT INTO franchise (name) VALUES (?)` <br> `INSERT INTO userRole (userId, role, objectId) VALUES (?, ?, ?)` |
| Close the franchise for t@jwt.com                   | closeFranchise.jsx | [DELETE] /api/franchise/${franchise.id}/store/${store.id} | `DELETE FROM store WHERE franchiseId=?` <br> `DELETE FROM userRole WHERE objectId=?` <br> `DELETE FROM franchise WHERE id=?`|

Now I know how to do yml files
