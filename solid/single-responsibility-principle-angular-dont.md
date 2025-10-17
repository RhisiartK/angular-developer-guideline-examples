```typescript
// Angular - Violation
@Component({
  selector: 'app-user-list',
  template: `...`
})
export class UserListComponent implements OnInit {
  users: User[] = [];
  
  constructor(private http: HttpClient) {}
  
  ngOnInit() {
    // Business logic (fetching)
    this.http.get('/api/users').subscribe(data => {
      this.users = data;
    });
  }
  
  deleteUser(id: string) {
    // Business logic (deletion)
    this.http.delete(`/api/users/${id}`).subscribe(() => {
      this.users = this.users.filter(u => u.id !== id);
      // UI logic (notification)
      alert('User deleted');
    });
  }
  
  exportToCSV() {
    // Data transformation logic
    const csv = this.users.map(u => `${u.name},${u.email}`).join('\n');
    // File handling logic
    const blob = new Blob([csv], { type: 'text/csv' });
    const url = window.URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'users.csv';
    a.click();
  }
}
```
