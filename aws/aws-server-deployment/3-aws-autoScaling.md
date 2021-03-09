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



### #2 CPU 사용 부하를 주어 auto scaling 기능 확인

- 다시 Auto Scaling 그룹 구성을 변경

  ![img](3-aws-autoScaling.assets/AZe1elO-qdM6OcdVXJ2GO7P_tPzyccxSs-3K6eU27EpIWiRB3HcUjN5-ZPOjw9CSJ6QKJZ8NpenVjbIPfscl0KvsshgzbRh1p4ozLwBNCHg9B-_Hh93whOC3BuPPTz2SSMHkCF9-)

- 인스턴스 실행을 확인

  ![img](3-aws-autoScaling.assets/gsV7aRRL0Fvl4tt5E806IAcReu3gtT6ybGKkje_u1SabD5dg9gGCXpdPHKnL-XsSm-pf7yh2F3fiGPVDU-5zzL_3EqfwuvdGDD_atDAgX2Nsz-ZMnevxcIHA3LL-Bpvtso6aL48c)

- 실행 중인 인스턴스에 SSH 접속

  ![img](3-aws-autoScaling.assets/OQOG5tpmk-HIEU2O0ZDm9XiHdPr2ZkK6R3-kwTeQjT1Ba3mIYpU2vVJgtH8qFdC9RgzGXZi6L7kTtL-vq1Cm1uE_qeS5iQ06WoMYImT9iONDnyR7AeAWAYe0ka40e0krJhj73z9y)

  ![img](3-aws-autoScaling.assets/i5rT7B2NFO_iZmSFuoqeL6eH1PI2tHbLvEoO6CeSZgYT2pR6xzhjdRASBHrd1RW0ZbAESmew97QqNPDJoebMtfUYWj_u9nnLd6kqgMBCy0cWsMghxNh5EfPb_feB6R_Hjn7WvAfI)

- stress  프로그램을 이용해서 CPU 사용량을 증가

  - $ `sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm`

  - $ `sudo yum install stress -y`
  - $ `stress --cpu 1 --timeout 600`

- 인스턴스의 증감을 모니터링

  - $ `top`

  - CPU 사용량 100%로 증가

    ![img](3-aws-autoScaling.assets/MIrCV5ASIfmsta1V0SAnXfRVnr58DIsjO9Irg52728nZb68UtqAF569-MV3yM_7W9Tp_eNnsWaAXPXWKthCILyb1yAmJvo7U64GRrbDGC7xKY_NM9dCPQHhHASzM5emy309I8YFJ)

  - 자동으로 인스턴스가 추가된 것을 확인

    ![img](3-aws-autoScaling.assets/VNP-97_TJ0f3OU8cRlVOYbuPe5LnndqT3o-RbX3_PCNL_rdt0mV_vkMl0-1S_dlOy3dLSR72Cm3_PWuKkyzEu9kV83oUmS3Ndd8bxw4CQ-DG3aa1ReO6h2Mz1H_B64gp1UUIKWgc)

  - 부하가 감소하면

    ![img](3-aws-autoScaling.assets/7CsDJ9RZdM1GGNpu0q9N0QkWJfc3VKxeGLz9IY79R2zeVX85_wYFdVjLNVLvnqLcB40iCau3b7U8LYWS5qNIgwbnlbo8EVuXG0YEFlWbcdcI3NbH__OnLCD7O7F93L4g6AsXBBVI)

  - 자동으로 인스턴스가 감소하는 것을 확인

    ![img](3-aws-autoScaling.assets/U9qi8vp1fBzelyHOyjKc4FgVj3wXkTB38a_QvMRHrI9tYUqzZpOmls54ht58qE4XUmezM4nUA8RgaxtuJmF2Qbw0-gEqud27mS8AIYn_JeTb0oMAfqMIivFIBUt45N8pCdZ5q7iw)



### #3 AWS ELB(Elastic Load Balancing)을 이용한 서버 트래픽 분산 관리(p62)

- 로드 밸런스 생성

  ![img](3-aws-autoScaling.assets/GjCM5x5qe-BGzDI9THh9XJutj7IKyIRiJvFgBwCGIlNvkbJeCI4LcNlyBoj45Ci4JDN7ZyQ22Nau9MLvnkEmIUp7EibrCLZDSMJD7v4S2w1hSysKOkNikeSG2agGGPwr8Q8_7me_)

  ---

  ![img](3-aws-autoScaling.assets/D4Uk6g7P3_3H8t9MPafZZpvabgscXQoZtxk3LOfbaJiRNjCYxCV_gKYy993OmQCQmtK0L_9LOPrZtMCPgxK5aq-GpoDT7alqB-tjS2LsSZrXpy-mAB__ouBmQQCsCgbn4PKlRJFk)

  ---

  ![img](3-aws-autoScaling.assets/zLSQ401otmb5KNyLeVADDk7DuBh0n4lpI9fGP3YVC-EW1gDcGXqvs27nv8jKfSFNvQgm60SBaInbm7Cn8SdD70xEYESLAjRsa7szdn-xzPuwBn8QCt2f9fL8-rPM9QP1j6AZ8AZi)

  ---

  ![img](3-aws-autoScaling.assets/iDFmOfQ3glk-ccp27YF8dxw5xyOTq4nwWNu2oYCXPPpf-Sxn2gNYrkL0OESc5EYHVfEZyJMOGJadF1ygCb5LeQKr_syjQnD-AfkuY_SNcZ5e1OG7zDS7QZub_ywfMHulDWMq3LwV)

  ---

  ![img](3-aws-autoScaling.assets/7JaLHkCsujRevOi6PDTczInfJ8BbL5Y2Jg2wJ4si5e7rxTqC3-Qd7o4d2YFuE4drLcMLuxWviCuzoOTdbP2RuUYb7OZQ2VfLXYUTgEJ_g3gOeYBKRKhKFT1eVIwxS94QCtCKehhx)

  ---

  ![img](3-aws-autoScaling.assets/oG3bmnO3KRJ1O-MOROJDzwnN5e3hhfAP9M1RIRpEXW5tjLB3R---50Ia-rTJckAM7-I35KOKWc7CBPopMEFiKxM7bErX5FOX2Y2ZKFYnL_-aGjE0SycCQ-CF-g4v0_6rKNwg0IYq)

- app.js 파일에 헬스 체크 URL 확인

  ```javascript
  app.get('/health', (req, res) => {					⇐ /health 로 GET 방식의 요청이 들어오면
    res.status(200).send(); 						   응답 상태코드의 값을 200으로 설정해서 반환
  });
  ```

- 대상 그룹에 Auto Scaling Group을 추가할 예정이므로, 인스턴스를 직접 추가하지 않는다.

  ![img](3-aws-autoScaling.assets/LD9c6aAhEPoVppcyerj57UPcRY1M50uOHnJOGGjvzMkAilpZIfeTOUpk0C1kGCNFVUdlwQeE0ogXaxcXt2y7Yc_jO-RDJYdQDy4hppdSu0juRWhjk2fVT16W6AXf7WH-tNk0f6MA)

- 로드 밸런스 생성

  ![img](3-aws-autoScaling.assets/slCuR5gkpyj7ntVs3lNBTYV0RG0FtQlMyGEJYiB57uoancn4Icfv4vsnRwgLRjh23KMZjOcr9jSgP8jA1gMkl73nZwmw0cgzQte7D7t_JUD_cHdJAFBxDSw8c_RrAJOyv2-6RWHk)



### #4 Auto Scaling 그룹을 로드 밸런싱 대상 그룹에 추가

- Auto Scaling Group에서 로드 밸런싱 편집

  ![img](3-aws-autoScaling.assets/t0a9XuMhklQE5HG--3Eov5YJrmUAB-EvsLjdB-_VQtph_Ny5FMO1ds0qHI7jhpz8GA4RQ-62yF4mxcNKZRBAMdj_kFMvqBL5CphwKGLvAQmruek01LyVWcV15cbEWbZ33GoA6j_l)

  ![img](3-aws-autoScaling.assets/yXpe4PzrxqVHRvYWDFYdFm9XkDUWzCE-633qEGBEH5cCH72r89ej2luAgMls-KcTmgZWrNVqEEPYEr3K2IraD2McBX0ghSPRlCFjBWFK2CGxUXrRF8hxg6iexuIn_wpP6QM7MWFr)

- 헬스 체크 확인

  ![img](3-aws-autoScaling.assets/2CkcRGvqnexgUamOI6rNEEqkAVJ6jl4cJeUd7Yt_IXZqPPgAqOlVl95zfQnERDGKVfIIc0H70ZCUQqjyvGHpQJVwcvk_iEJlYjMNj8dzEeGp_sVw3Zkxi4wZOC4mwb-Qddb7A6Wn)

  ---

  ![img](3-aws-autoScaling.assets/RNjwSWC4Syt8ySRpV9yGK58rVkFbT0WueVYY5p09IIBpq9mfSQqUDFfKfNDVeAXFuJ2dcktzPBcUcg0bgv8bQ9cf8kpITRODlPffXSr-WRuGQl71017YBS0wUcQspPhoRUZMAF1e)

  ---

- 로드 밸런스를 통한 접속 확인

  ![img](3-aws-autoScaling.assets/cNOHoA7hkCdjiASbmOAnlD6-B9JxWAVfX53rjJAj40O-mr7IV59VpiMwmNRjHI_TnB__SX8fIInuS-p3oopGKM8SRJCVW4WE8QgHUG57MYtkrDQb80h_RAR-AL-Hev6a4Tohr0yD)

  ---

  ![img](3-aws-autoScaling.assets/QKKiZcHDLbPVCUkMNFyDvPX8RiKFZgW5ZK9kcEMRud2ePr2lyOUGQDoF0Q8QIjws8b0bATV-RooinKUd8wuRqrjpTpXQiLxljvhJBf66JoJg4Btdsg_CqiPwI3IDpxm1VGmQB0zs)

  

### #5 ASG(Auto Scaling Group)과 ELB(Elastic Load Balancer)를 이용한 장애 조치 (p73)

- 대상 그룹의 헬스 체크 설정 변경

  ![img](3-aws-autoScaling.assets/O4K1-kxB_-GtsCumzbNr4dYKn0EtkJKP3AhAzbDhc2gI2z0fFLgvsyGjkqKl6aPBDr1ux4DiOL1usQrqtElrPq3M8gwSXYRI4_6lsxCGWvpMgIXoslHV_7IWh89e0R77SwjSPX-5)

  ---

  ![img](3-aws-autoScaling.assets/n9GQvq8jbfstNqCPDKG2xtE7_HxU2yVDXtKuRjhfY2Dtetz-6YWSmQiFw4Oc9jSVy5db-ufbaF0nEgSvTHt7jXji9JGWyOwwAhzAjaYiQ1cl5HL_dqgFicAurYRRjTyZvel2SNQ1)

  ---

  ![img](3-aws-autoScaling.assets/NZQEXYNie3_AexU1faHULrK6JoeBiHvU3BuY38XRADNah3aQAYVxV5GVrxMGMv8x0pm8kW0J2AxyOyqYOHijt9pVag1JZtZlS40rq-tlbnFpzg3H8gCepDYpptj7hKDHJPxiosIr)

  ---

  ![img](3-aws-autoScaling.assets/G_7-2KFwG8-SlSysjdmJRic8zOg-Nm5mu_KmRpH6Fi03HQF8kzvq9BHWq6F-niuyzCrnr-iLv7KVwmWBg3z2LUQOX92HGKuMGPilRSF9xxJufxfr3giZSDvYt9HJcFviepSasy-e)

- ASG의 용량을 변경

  ![img](3-aws-autoScaling.assets/JsqDEggfRRbCTC0LWggvvLm58X2sJpn3_jhfIeFDfOUd9vKira8xcrOnAjJqPslY3gMDtN1THJ0cQVqBclkZ7CCD48WacpXmw3oyjdkiJKL9u2EutgcgPn1pEzz7VGnF9fhJquFh)

  - 항상 인스턴스가 2개 실행되도록 설정을 변경

  ![img](3-aws-autoScaling.assets/S3JOpGdmmgmUEaEEpb9swl1f3Q3TTJ0ltL27Gbyeiugr6RR2GuE6BSI-bN84NUTNCgaE6xBjeMpG4oLOH67jJi-UcUNiA3RYZH6Zhksk4e-oeVWzLrvkDMeatSvS8WuVQt_fIF1t)

- 각 인스턴스로 SSH 접속해서 헬스 체크 확인

  ![img](3-aws-autoScaling.assets/bYXeSxkkKi7oozNR5mMSIfaMxoXwWvamJ4DKnR0FVol9gk_NlB-XS9uee09MG7NTjgqS4M9USjA7DKNNOVUO5n-9EnorPSiWGkkMieR-D14K8JpRSx5fFZrCWJwImYnZ6mrK3G3f)

  - /opt/nginx/logs/access.logs 파일을 통해 헬스 체크 요청이 5초 간격으로 발생하는 것을 확인

    - `tail -f /opt/nginx/logs/access.logs ` 

    ![img](3-aws-autoScaling.assets/SmfP3b_8Onc6FoMUcK1Td2rOnoleeXdfwc36r1Qf8V7tjOy_q1OXmDw5ppBnO9VG_fiKOh3FmYabiDl8GjZB755dsBMMWQHL9DP1bHupBPO6wV-aCuX191qa6__xZvPosgyGoEDf)

    

- 인스턴스 중 하나의 nginx 서비스를 중지

  - $ `sudo service nginx stop`

  - nginx 서비스 중지와 동시에 로드밸런서 퍼블릭 DNS 주소로 접속을 시도하면 502 Bad Gateway 오류가 발생

    ![img](3-aws-autoScaling.assets/JAG_aQOkD6Gbd6GOc26EQWB1_L27F1NG930cLeZA1RqsyC5ve5CQBLv9HzBTR1vU8RPoEV9xAsOIzRb5zPwsAMd1CAGl9HlsGqNZ6qJUa5mTwqgiZB0qKece6vM5C2g8h47icUbY)

  - 시간이 지난 후에는 정상적으로 동작하는 것을 확인

    - ⇒ 정상적으로 동작(healthy)하는 인스턴스로만 요청을 전달

      ![img](3-aws-autoScaling.assets/08bx3Fn7BQ0L3Ib4x-sB9NQqMZ-DWeRfYu9Jh6Uq4LgN5REW7i_Ic-2NBHXQWV29p76PmgkPwCVDZtyhT9r9HzzK7ZPIXePtyGcdfbCQvecOqzowah5ejKIGebxUeheFZLJ9aBp6)

    ![img](3-aws-autoScaling.assets/fN66iKEb14rWDnqMDySlRi8jfsVZWkpVGehiEN678QTmq_b2E6XCXo5hTO4I5PXP25I68f362CULP0YdLCpN_zm4BFu1RKlWKObHvWjYjzm3E0GY3hUD8hhNDwL1RgsOLs0cD_qD)

- 모든 인스턴스를 종료 

  ![img](3-aws-autoScaling.assets/krL2PKuxbgwkydRVZpnJe6wEc6UUe6jccxC5iWfup4Kczl4vU8nHdq3gdNQwrxeGeto1LkP6MO0j3JjZuoJFZ8youHj0t1Wkw1IXAACePdtKofMC4-nxHIUQ85QM_JPCfI-xWbfl)



