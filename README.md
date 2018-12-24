package khoapham.ptp.phamtanphat.firebaseauthenticationandroid;

import android.net.Uri;
import android.support.annotation.NonNull;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

import com.google.android.gms.tasks.OnCompleteListener;
import com.google.android.gms.tasks.Task;
import com.google.firebase.auth.AuthResult;
import com.google.firebase.auth.FirebaseAuth;
import com.google.firebase.auth.FirebaseUser;
import com.google.firebase.auth.UserProfileChangeRequest;

public class MainActivity extends AppCompatActivity {

    Button btnDangnhap, btnDangky , btnCapnhat , btnXacthuc , btnSetPassword;
    EditText edtEmail, edtPassword;
    String email, password = "";
    private FirebaseAuth mAuth = FirebaseAuth.getInstance();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btnDangky = findViewById(R.id.buttonDangky);
        btnDangnhap = findViewById(R.id.buttonDangnhap);
        btnCapnhat = findViewById(R.id.buttonCapnhat);
        edtEmail = findViewById(R.id.edittextEmail);
        edtPassword = findViewById(R.id.edittextPassword);
        btnXacthuc = findViewById(R.id.buttonXacthuc);
        btnSetPassword = findViewById(R.id.buttonSetPassword);

        btnDangky.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                getTaikhoan(edtEmail, edtPassword);
                mAuth.createUserWithEmailAndPassword(email, password)
                        .addOnCompleteListener(MainActivity.this, new OnCompleteListener<AuthResult>() {
                            @Override
                            public void onComplete(@NonNull Task<AuthResult> task) {
                                if (task.isSuccessful()){
                                    Toast.makeText(MainActivity.this, "Dang ky tai khoan thanh cong!!!", Toast.LENGTH_SHORT).show();
                                }else {
                                    Log.d("BBB",task.getException().getMessage());
                                }
                            }
                        });
            }
        });
        btnDangnhap.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                getTaikhoan(edtEmail, edtPassword);
                mAuth.signInWithEmailAndPassword(email, password)
                        .addOnCompleteListener(MainActivity.this, new OnCompleteListener<AuthResult>() {
                            @Override
                            public void onComplete(@NonNull Task<AuthResult> task) {
                               if (task.isSuccessful()){
                                   Toast.makeText(MainActivity.this, "Dang nhap thanh cong!!", Toast.LENGTH_SHORT).show();
                               }else {
                                   Log.d("BBB",task.getException().getMessage());
                               }
                            }
                        });
            }
        });
        btnCapnhat.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                FirebaseUser user = FirebaseAuth.getInstance().getCurrentUser();

                if (user != null){
                    UserProfileChangeRequest profileUpdates = new UserProfileChangeRequest.Builder()
                            .setDisplayName("Pham Tan phat")
                            .setPhotoUri(Uri.parse("https://www.freeiconspng.com/uploads/christmas-claus-noel-reindeer-santa-icon-24.png"))
                            .build();

                    user.updateProfile(profileUpdates)
                            .addOnCompleteListener(new OnCompleteListener<Void>() {
                                @Override
                                public void onComplete(@NonNull Task<Void> task) {
                                    if (task.isSuccessful()) {
                                        Toast.makeText(MainActivity.this, "Cập nhật tài khoản thành công", Toast.LENGTH_SHORT).show();
                                    }else {
                                        Log.d("BBB",task.getException().getMessage());
                                    }
                                }
                            });
                }

            }
        });
        btnXacthuc.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                FirebaseAuth auth = FirebaseAuth.getInstance();
                final FirebaseUser user = auth.getCurrentUser();

                if (user != null){
                    user.sendEmailVerification()
                            .addOnCompleteListener(new OnCompleteListener<Void>() {
                                @Override
                                public void onComplete(@NonNull Task<Void> task) {
                                    if (task.isSuccessful()) {
                                        Toast.makeText(MainActivity.this, "Gửi xác thực thành công", Toast.LENGTH_SHORT).show();
//                                        Log.d("BBB",user.isEmailVerified() + "");
                                    }else {
                                        Log.d("BBB",task.getException().getMessage());
                                    }
                                }
                            });
                }
            }
        });
        btnSetPassword.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                FirebaseUser user = FirebaseAuth.getInstance().getCurrentUser();
                String newPassword = "456789";

                if (user != null){
                    user.updatePassword(newPassword)
                            .addOnCompleteListener(new OnCompleteListener<Void>() {
                                @Override
                                public void onComplete(@NonNull Task<Void> task) {
                                    if (task.isSuccessful()) {
                                        Toast.makeText(MainActivity.this, "Cập nhật mật khẩu thành công!!", Toast.LENGTH_SHORT).show();
                                    }else {
                                        Toast.makeText(MainActivity.this, "Thất bại", Toast.LENGTH_SHORT).show();
                                    }
                                }
                            });
                }

            }
        });
    }

    private void getTaikhoan(EditText valuemail, EditText valuepassword) {
        email = valuemail.getText().toString();
        password = valuepassword.getText().toString();
    }
}
