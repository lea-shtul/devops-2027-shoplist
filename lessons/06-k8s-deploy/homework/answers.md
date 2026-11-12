תשובה שאלה 1:
השמות ש-Compose נותן נקבעים לפי שם התיקייה (docker) ושם הסרוויס, ולכן הם תלויים במקום ההרצה ולא ברורים. נתנו שם מפורש ויציב כדי שה-YAML יוכל להפנות אליו בדיוק. בלי imagePullPolicy: Never, אם ה-image לא נמצא במחשב, Kubernetes היה מנסה להוריד אותו מ-Docker Hub, נכשל, ומקבלים ImagePullBackOff. עם Never הוא לא מנסה להוריד בכלל, ואם ה-image חסר רואים ErrImageNeverPull.


תשובה שאלה 2:
לא למדנו


שאלה 3:

ה-Deployment (replicas: 1) הרים pod חדש במקום זה שנמחק, אבל הקובץ שאחראי לכך שהמוצרים נשארו הוא postgres-pvc.yml: הוא יוצר דיסק קבוע שקיים בנפרד מה-pod, וה-pod החדש מתחבר אליו ומוצא את הנתונים. kubectl delete pvc postgres-pvc -n shoplist הייתה מוחקת אותם.

שאלה 4:

ב-Kubernetes כל ה-pods עולים במקביל, בלי סדר. אם backend עולה לפני postgres, בקשות שמגיעות לפני שה-DB מוכן נכשלות, אבל כי backend מתחבר מחדש בכל בקשה, ברגע ש-postgres מוכן הכל עובד (ואפליקציה שהייתה נופלת, Kubernetes היה מפעיל מחדש). מה שדואג שלא יגיעו בקשות לפני הזמן הוא ה-readinessProbe: ה-Service שולח רק ל-pods שעברו אותו, pg_isready ב-postgres ו-/health ב-backend.

שאלה 5 - לא למדנו

שאלה 6

משתני סביבה שמגיעים מ-ConfigMap נקראים רק פעם אחת, כשה-pod עולה. apply מעדכן את ה-ConfigMap ב-cluster, אבל ה-pods שכבר רצים ממשיכים עם הערך שקיבלו. הפתרון הוא kubectl rollout restart deployment/backend -n shoplist, שמחליף את ה-pods בחדשים שקוראים את הערך המעודכן.

שאלה 7:

תבתי shoplist __ פעמים (12 מהן הן ה-namespace, והשאר בשמות ה-images ובשם ה-DB). כדי להריץ עותק ב-shoplist-test צריך לשנות את כל 12 הקבצים, וגם את ה-nodePort ב-frontend-service, כי 30080 כבר תפוס ב-cluster. זה מראה שכשאותו ערך מועתק להרבה קבצים קל לטעות, ולכן צריך כלי שמגדיר אותו פעם אחת.