## AWS Auto Scaling 그룹을 이용한 다중 서버 구성

> AWS 인프라 구축 가이드 p44

### #1 Auto Scaling 그룹 생성(p48)

- 인스턴스 중지

  ![img](3-aws-autoScaling.assets/cFQs1oN-virGMMH3CiNY_uRz1lJNIfGTxc1bXbgLfiPmfxnI_VOSTCSxBgpt5b5NFZiL8AE4UKYN8oxkWNBdsFdxDPdSL-0ryZUyEgQpiRwbBfjvMhdna9DRMlh-Ag6uo5AfWm-h)

- 스냅샷 생성

  ![img](3-aws-autoScaling.assets/sIk9KB69n900Ug8k8SWGWhBC4I9KwpdyTu-_Qgh68fbDHQ3lh4aIdHTI6jZVXvgUrCl0rA-_JK31DFeslB-nh4eZ0l5XfTbj_C1NJ0duN_AWFUxdo4xRribQ_Di_lX-sBaQ1CqnF)

  ---

  ![img](3-aws-autoScaling.assets/XMu_VwnlsD6a1jFb1sQ8hh9cDm1hTIvTruu9-JUHRLmlHPh5pM-c2v-8d2OhC0RJ37LAYeDwAi700cOQ6Kr1J-_bWDp6R6PwUzGXot1xbg8kHgd0wvBrIGz0wPpDwPIeGxLTQu94)

- 시작 템플릿 생성

  ![img](3-aws-autoScaling.assets/2oS1uUnPo7TWHRwPsGGwZPIRIok7J2_W_FIhuPdAdjplCwzL94HacPbhAaUrlMOiPv-kkJ8GW5WjmsMY30tNMiX06m8iDS1usilsoxKWUxEZfB-nAkYTVdmjnRVh_krqBEPyOXZB)

  ---

  ![img](3-aws-autoScaling.assets/Gk7YbJULFIqXPMKzvRitbq1_o51JYaF4SUOF8FGAyU6h3Gn1bQ8yAH1JytAGlVLGwSBsx7Sc4EW4ptgJVVM_jCg8e8TlXuCi15ze4HD_C7ZxQBoRZWSuyocOAbpq3Mxw3oNHa_RQ)

  ---

  ![img](3-aws-autoScaling.assets/AkUzIhAcTDWUaXXtP7prZJv9gcTYMYpVI2XDrwYX5F4qOT8o6BehDpTIm4NFsmZVIPY-WfF84VFvNgRDFgWzM3S8B643m9aC3D4cqnSB9NheqHAjWF-AA19gJf_1TNPoO1pnwONC)

  ---

  ![img](3-aws-autoScaling.assets/hM7M04MqouqUWlE1YkZcCGQFVhSASyLL3srvFurnogXMPH9VRnvby2CJR__QvmZQ_S7h1mmgiuX8AvyZRkPo_k0f6m0oxiBqpXeBFEEWDt8-astw5yEGNR5WXKJa38Brgh7s0x5A)

  ---

  ![img](3-aws-autoScaling.assets/l3dGjwzFwIZ8wKmHxpnV9erRFDBrAqKJK8yJ8HF3AKRb9V7JNZukP7DtKK21y4BK86TJc4Ya88VqmxkbRuAsUva2Yftoj9d26o9wHqXmKkWvTQloMtk6RH9SWZ9jB-M9g9mp-lpC)

- Auto Scaling 그룹 생성

  ![img](3-aws-autoScaling.assets/6Z3PyuPYRLw7-EXLorPjoW4Od2311tVoRYcveyj_MxgdZMjfrXSQVR08e2nyAkOkKwV7lvKMhn0TXfBCZqFYH2-5es1YzofcGN1SZ-r3U7OUt8g5a0eNrFP56mS7PcjPY7HvcuE4)

  ---

  ![img](3-aws-autoScaling.assets/G-JpGpzVBJhpJ-zDA5VyIlE6FN2Xzxb1-h2ssjkArlT2gHZ3tQWUBPMd65xp3v5MxT1STDV_eqsczptSnyKvbvzwSX903gH6ee96VpnHgmpBxB6DeBA9n9JqDI6QY_o3sqa2FacT)

  ---

  ![img](3-aws-autoScaling.assets/HYmTeN-jFCzMUzfUwYnBg6ZUrt4BOc1qbVDlVa_utokBRpBH_LKcUXxkojvm_c0onk_nmF-9HIbti8TGEh6KYxtVSgPu91MRQFkfmkaCxXvFxf2dUF14HFsZnnzUydnFkE1RZsmZ)

  ---

  ![img](3-aws-autoScaling.assets/4vmb7LDxLv_jFGgLpVUd4F-lCj095xuw4MFF7ZHxLrjelKjY_dz5ufMvgJxJfrRtOKMybeZnhIjUwlrK4QjDtWt_UVH9yyoUNTsIa92FeCVnd6AewdPbl7NaX10n0d13GnOXjtfL)

  ---

  ![img](3-aws-autoScaling.assets/6qYVhdxM8-fAqbjoLroSOZjwt1xOn7VymwnIHG1g-F-DpfBgGCRI2dxxM-BfE0xebCk9NNJK7XB4junvT5LDSQg4826Fmf8r2wbK7mNpfC_8axEk1z6yQBeVEqN1pznzAvDun7lk)

  ---

  ![img](3-aws-autoScaling.assets/EfA20ylDcn01woChqj02eeCV6GWq2iqMiJ9VcTNSncOvnWCUMpI5knuxLGmRBc6WHB0gUU6yxZT9idjbDZtab4EX49JD-FOsIQyBJQBEgz0-kphTD9c6CeVHjuwnE1PP3AlAPy2S)

  ---

  ![img](3-aws-autoScaling.assets/BdL6E5ZGM35Fh7wKuZhxqGWf7zDcX37MI21F55AYW1AiSnchn59dEV_XvzG5uReNMho8DYsUaLgyuBb9JEihLHAbRAQOP6J8Gb0PVMhPiYbRjoDvq1LsH3pQN3-KyRvORw4Lpfcc)

  ---

  ![img](3-aws-autoScaling.assets/4IdBVmMiqs1h1XNPMcX7ECMCdv-P6r_O2Whjzg6P56D0896zexyAhfaDhjBS3yX5wyuXR_MahA1Pm6i_X0ulGp-CYnxcwkXSvLTCa8-sfvH_4rlb_An-erjSGK6FmoxqJPhnFK-6)

  ---

  ![img](3-aws-autoScaling.assets/b_wjQQrMNfM8nZT-w5Ydt3WK0_wf9KBp2dw6PGu3sgzAqcGRxg3eMgMwVlWxQj1cpBB_tnQiA9Cub_EBxyUK7YfYf9BZ3h3Gf5BwBTK2vy_dD3Jyc86emHmQhNMS9xD5OnJ07KtA)

  ---

  ![img](3-aws-autoScaling.assets/6PCmkxa8Ejs7yuJ0ShRapsxFAG0WhPZ9ydv6vdquxL9ngrJWrBbf9U16fv_yrYgJnF_N3C7ScnN_PALAhPPavIRaE0FJyYz7dhfVkt9uzp-jhTg4djl6jKWV7xeg7WZ1RH_ncSYU)

  ---

  ![img](3-aws-autoScaling.assets/6PCmkxa8Ejs7yuJ0ShRapsxFAG0WhPZ9ydv6vdquxL9ngrJWrBbf9U16fv_yrYgJnF_N3C7ScnN_PALAhPPavIRaE0FJyYz7dhfVkt9uzp-jhTg4djl6jKWV7xeg7WZ1RH_ncSYU)

  

- Auto Scaling Group 설정에 인스턴스를 모두 종료

  ![img](3-aws-autoScaling.assets/i5fcweBfRoRYQUq5o9ZE2005lc9__Fin4EGo4URKKtN5uLUgWmj5QbPishB2Ydh_zFNoNuF17opPHH0iC3juziQZPm3nOrffPs3tYs1sn0obSCdRXiF4Qb0bDE1XsObkctx0R03P)