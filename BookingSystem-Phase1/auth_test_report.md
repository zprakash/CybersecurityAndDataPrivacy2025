# Authorization Test Report

## Application Specification Reference

The following specifications are used as the baseline for testing:

1. The system is accessed via a web browser  
2. Users can register and log in  
3. Logged-in users act as either a resource reserver or an administrator  
4. Administrators can add, remove, and modify resources and reservations, and delete reservers  
5. Reservers can book a resource if they are over 15 years old  
6. Resources can be booked on an hourly basis  
7. Booked resources are visible without login, without revealing reserver identity  
8. The system must comply with GDPR and Privacy by Design principles  

---

## Guest (Not Logged In)

### ✅ Can do

- Can access landing page — `/`  
  Observation: Landing page loads successfully without authentication  
  Spec: ✔️ Matches spec (point 7 – public visibility of bookings)

### ❌ Cannot do / Issues

- Cannot access resources page properly — `/resources`  
  Observation: Resource creation form is displayed instead of a read-only resource list; submitting the form redirects to `/` without creating a resource  
  Spec: ❌ Violates spec (points 4 & 7 – resource management must be admin-only; guests should only see booked resources)

- Cannot submit resource creation request — `POST /resources`  
  Observation: Submitting the form sends a POST request to `/resources` which returns `302 Found` and redirects to landing page without authorization error  
  Spec: ❌ Violates spec (point 4 – only administrators may add resources)

- Cannot access reservation API — `GET /api/reservations`  
  Observation: Guest receives full reservation data in JSON, including reservation IDs, resource IDs, names, and start/end times, without authentication  
  Spec: ❌ Violates spec (points 3 & 7 – guests should only see public resources; reservation details should not be exposed)

- Cannot access reservation creation page — `/reservation`  
  Observation: Access blocked; status page displays “Unauthorized” with a back-to-home button; no reservation functionality is accessible  
  Spec: ✔️ Matches spec (point 3 – only authenticated users may reserve resources)

- Cannot access profile page — `/profile`  
  Observation: Access blocked; status page displays “Not Found” with a back-to-home button  
  Spec: ✔️ Matches spec (point 3 – only authenticated users can access their profile)

- Cannot access admin dashboard — `/admin`  
  Observation: Access blocked; status page displays “Not Found” with a back-to-home button  
  Spec: ✔️ Matches spec (point 4 – only administrators may access admin pages)

- Cannot access any admin subpages — `/admin/*`  
  Observation: Access blocked for all `/admin/*` URLs; status page displays “Not Found” with a back-to-home button  
  Spec: ✔️ Matches spec (point 4 – only administrators may access admin pages)

- Cannot access users API — `GET /api/users`  
  Observation: Guest receives full user list in JSON, including usernames, roles, and user tokens; no authentication required  
  Spec: ❌ Violates spec (point 4 – guests should not access any user data; only administrators should)

- Cannot access reservation details page — `/reservation?id=3`  
  Observation: Guest is blocked and shown a status page (“Unauthorized” / “Not Found”); no reservation details are exposed  
  Spec: ✔️ Matches spec (Point 3 & 5 – only authenticated users can interact with reservations)


---

## Reserver (Authenticated User)

### ✅ Can do

- Can log in successfully — `/login`  
  Observation: Login succeeds with reserver credentials; user is redirected to their dashboard/profile  
  Spec: ✔️ Matches spec (point 2 – registered users can log in)

- Can view resources page — `/resources`  
  Observation: Resource creation form is displayed; clicking save sends a POST request to `/resources` which returns `302 Found` and redirects to dashboard  
  Spec: ⚠️ Not defined in spec (point 4 – only administrators should manage resources; reserver should likely only view resources)

- Can access reservation page — `/reservations`  
  Observation: Reservation form is displayed; user can fill in and submit a reservation  
  Spec: ✔️ Matches spec (point 5 – reservers over 15 years old can book resources)

- Can register as a reserver — `/register`  
  Observation: Registration form accepts new user details; system validates age and only allows registration if user is over 15; otherwise shows error message “You are not above 15”  
  Spec: ✔️ Matches spec (point 5 – only users over 15 can reserve resources)

- Can access specific resource details — `GET /api/resources/:id`  
  Observation: Reserver receives limited resource information in JSON (resource ID, name, description); no admin-only fields are exposed  
  Spec: ✔️ Matches spec (point 7 – reservers can view resource details)

- Can access own reservation details — `/reservation?id=<own_id>`  
  Observation: Reserver can view reservation details only when the reservation belongs to them  
  Spec: ✔️ Matches spec (Point 5 – reserver can manage their own bookings)

- Can update own reservation — `/reservation?id=<own_id>`  
  Observation: Update action succeeds only for reservations created by the same reserver  
  Spec: ✔️ Matches spec (Point 5 – reserver manages own reservations)

- Can delete own reservation — `/reservation?id=<own_id>`  
  Observation: Delete action succeeds only for own reservations  
  Spec: ✔️ Matches spec (Point 5 – reserver manages own reservations)

### ❌ Cannot do / Issues

- Can access all reservations via API — `GET /api/reservations` and `GET /api/reservations/:id`  
  Observation: Reserver can see all reservations in JSON, including reservations created by other users; no restriction is applied based on ownership  
  Spec: ❌ Violates spec (points 3 & 7 – reservers should only access their own reservations; exposing all reservations is unauthorized)

- Cannot access own profile page — `/profile`  
  Observation: Access is blocked; status page displays “Not Found” with a back-to-home button  
  Spec: ⚠️ Not defined in spec (expected: reservers should see their own profile; current behavior blocks access)

- Cannot access admin dashboard — `/admin`  
  Observation: Access is blocked; status page displays “Not Found” with a back-to-home button; HTTP 302 redirect to home  
  Spec: ✔️ Matches spec (point 4 – only administrators may access admin pages)

- Cannot access admin user list — `/admin/users`  
  Observation: Access is blocked; status page displays “Not Found” with a back-to-home button; HTTP 302 redirect to home  
  Spec: ✔️ Matches spec (point 4 – only administrators may access admin pages)

- Cannot access resources API — `GET /api/resources`  
  Observation: Access is blocked; status page displays “Not Found” with a back-to-home button; HTTP 404 returned  
  Spec: ✔️ Matches spec (point 4 – only administrators may access resource management APIs)

- Can access users API — `GET /api/users`  
  Observation: Reserver receives full user list in JSON, including usernames, roles, and user tokens; no restriction based on role  
  Spec: ❌ Violates spec (point 4 – reservers should not access other users’ data; only administrators should)

- Cannot update other users’ reservations via UI — `/reservation?id=<other_id>`  
  Observation: UI blocks access; no form or buttons appear  
  Spec: ✔️ Matches spec (frontend restriction enforced)

- Can update other users’ reservations via direct URL — `/reservation?id=<other_id>`  
  Observation: Direct URL allows updating reservations of other users; backend authorization missing  
  Spec: ❌ Violates Point 5 (reservers should not update others’ reservations; IDOR vulnerability)

- Cannot delete other users’ reservations — `/reservation?id=<other_id>`  
  Observation: Delete action is blocked even via direct URL; backend prevents deletion  
  Spec: ✔️ Matches spec (prevents horizontal privilege escalation)


---

## Administrator

### ✅ Can do

- Can register and log in as administrator — `/login` + `/register`  
  Observation: Admin registration and login succeed; admin is redirected to admin dashboard  
  Spec: ✔️ Matches spec (point 2 – registered users can log in; point 4 – admin role)

- Can access resource form via `/resources`  
  Observation: Resource creation form is displayed; admin can fill in and submit to create a resource  
  Spec: ✔️ Matches spec (point 4 – admin can add resources)

- Can access reservation API — `GET /api/reservations`  
  Observation: Admin receives full reservation data in JSON, including all reservations from all users  
  Spec: ✔️ Matches spec (point 4 – admin can view/manage all reservations)

- Can access users API — `GET /api/users`  
  Observation: Admin receives full user list in JSON, including usernames, roles, and user tokens  
  Spec: ✔️ Matches spec (point 4 – admin can view all users)

- Can access reservation details — `/reservation?id=3`  
  Observation: Administrator can view reservation details for any reservation, regardless of owner  
  Spec: ✔️ Matches Point 4 – admin can manage all reservations

- Can update any reservation — `/reservation?id=3`  
  Observation: Administrator can update reservations created by any reserver via UI or direct URL  
  Spec: ✔️ Matches Point 4 – full reservation management

- Can delete any reservation — `/reservation?id=3`  
  Observation: Administrator can delete reservations created by any user via UI or direct URL  
  Spec: ✔️ Matches Point 4 – full reservation management

### ❌ Cannot do / Issues

- Cannot access admin resource creation page — `/admin/resources/new`  
  Observation: Status page shows “Not Found” with back-to-home button; UI link missing, but admin can still create resources via `/resources`  
  Spec: ⚠️ Not defined in spec (UI inconsistency — admin functionality exists but the dedicated admin page is unavailable)

- Cannot delete a reserver — `/admin/users/delete/:id`  
  Observation: Status page shows “Not Found” with back-to-home button; admin cannot delete users via the UI/API  
  Spec: ⚠️ Not defined in spec (expected: admin should be able to delete users; current behavior prevents it)

- Cannot manage all reservations — `/admin/reservations`  
  Observation: Status page shows “Not Found” with back-to-home button; admin cannot access reservation management UI  
  Spec: ⚠️ Not defined in spec (expected: admin should be able to view/manage all reservations)

