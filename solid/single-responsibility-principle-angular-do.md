```typescript
// Service - API Communication
@Injectable({ providedIn: 'root' })
export class UserService {
  constructor(private http: HttpClient) {}
  
  getUsers(): Observable<User[]> {
    return this.http.get<User[]>('/api/users');
  }
  
  deleteUser(id: string): Observable<void> {
    return this.http.delete<void>(`/api/users/${id}`);
  }
}

// Utility - Data Transformation
export class UserExportUtility {
  static toCSV(users: User[]): string {
    return users.map(u => `${u.name},${u.email}`).join('\n');
  }
}

// Service - File Export
@Injectable({ providedIn: 'root' })
export class FileExportService {
  downloadFile(content: string, filename: string): void {
    const blob = new Blob([content], { type: 'text/csv' });
    const url = window.URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = filename;
    a.click();
  }
}

// Component - UI Rendering Only
@Component({
  selector: 'app-user-list',
  template: `...`
})
export class UserListComponent implements OnInit {
  users$ = this.userService.getUsers();
  
  constructor(
    private userService: UserService,
    private fileExportService: FileExportService,
    private notificationService: NotificationService
  ) {}
  
  onDeleteUser(id: string): void {
    this.userService.deleteUser(id).subscribe(() => {
      this.notificationService.show('User deleted successfully');
    });
  }
  
  onExportCSV(): void {
    this.users$.subscribe(users => {
      const csv = UserExportUtility.toCSV(users);
      this.fileExportService.downloadFile(csv, 'users.csv');
    });
  }
}
```
