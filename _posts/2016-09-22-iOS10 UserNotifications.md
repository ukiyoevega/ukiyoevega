---
layout: post
title:  "iOS10 UserNotifications"
date: 2016-09-23 21:06:26 +0800
categories: iOS
description: iOS10 UserNotifications学习笔记

---

### 第一步，请求用户授权使用Notification发送通知。
一般在viewDidLoad方法中请求。远程通知和本地通知都通过词方法授权。注意：通知中心不能自行创建，current()用于获取应用默认的通知中心。

```swift
UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound]) { (granted, error) in
    if granted {                self.loadNotificationData()
    } else {
        print(error?.localizedDescription)
    }
}
```

### 第二步，推送本地通知。
创建一个通知对象，用于demo单击按钮对应展示。trigger设置通知展示时间长，是否重复的属性，有三种类型。UNTimeIntervalNotificationTrigger，UNCalendarNotificationTrigger，UNLocationNotificationTrigger。iOS10中的通知可以通过atttachments添加图片、视频、音频等的附件。但附件都必需下载到本地。通过trigger和content建立request请求，请求具有唯一标识，将请求加入通知队列。delegate接收到这个通知请求对应的response。
```swift  
func scheduleRandomNotification(inSeconds: TimeInterval, completion: @escaping (_ success: Bool) -> ()) {
    let content = UNMutableNotificationContent()
    content.title = "New cuddlePix!"
    content.subtitle = "What a treat"
    content.body = "Cheer yourself up with a hug 🤗"
    let randomImageName = "hug\(arc4random_uniform(12) + 1)"
    guard let imageURL = Bundle.main.url(forResource: randomImageName, withExtension: "jpg") } else {
      completion(false)
      return
    }
    let attachment = try! UNNotificationAttachment(identifier: randomImageName, url: imageURL, options: .none)
    content.attachments = [attachment]
    let trigger = UNTimeIntervalNotificationTrigger(timeInterval: inSeconds, repeats: false)
    let request = UNNotificationRequest(identifier: randomImageName, content: content, trigger: trigger)
    UNUserNotificationCenter.current().add(request, withCompletionHandler: { (error) in
        if let error = error {
            print(error)
            completion(false)
        } else {
            completion(true)
        }
    })
}
```

### 第三步，为应用添加前景通知。
默认的本地通知只能在应用处于后台或者未被启动状态时推送给用户，如果应用已经位于前台正在运行，想要在这个时候推送通知，要通过特定的方法设置。并在application(_:didFinishLaunchingWithOptions:)中把NotificationHandler的实例赋值给UNUserNotificationCenter的 delegate属性。UNUserNotificationCenter.current().delegate = self
```swift      
func userNotificationCenter(_ center: UNUserNotificationCenter, willPresent notification: UNNotification, withCompletionHandler completionHandler: (UNNotificationPresentationOptions) -> Void) {
    //No.8 ？
    NotificationCenter.default.post(name: userNotificationReceivedNotificationName, object: .none)
    completionHandler(.alert)
}
```

### 第四步，对通知进行响应。
通过将一系列动作放到一个category中，将这个 category 进行注册，将通知的 category 设置为要使用的 category 来实现的。构造完响应动作以后，在程序启动时的application(_ application:didFinishLaunchingWithOptions:)中调用这个方法进行注册。
在完成 category 注册后，发送一个 actionable 通知就非常简单了，只需要在创建 UNNotificationContent 时把 categoryIdentifier 设置为需要的 category id就好了。content.categoryIdentifier = newCuddlePixCategoryName.
```swift    
func configureUserNotifications() {
    let starAction = UNNotificationAction(identifier: "star", title: "🌟 star my cuddle 🌟 ", options: [])
    let dismissAction = UNNotificationAction(identifier: "dismiss", title: "Dismiss", options: [])
    let category = UNNotificationCategory(identifier: newCuddlePixCategoryName, actions: [starAction, dismissAction], intentIdentifiers: [], options: [])
    UNUserNotificationCenter.current() .setNotificationCategories([category])
}
```

这个代理方法会在用户与你推送的通知进行交互时被调用，包括用户通过通知打开了你的应用，或者点击或者触发了某个action。通过 request 中包含的 categoryIdentifier 和 response 里的 actionIdentifier 就可以轻易判定是哪个通知的哪个操作被执行了。
```swift  
func userNotificationCenter(_ center: UNUserNotificationCenter,didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: () -> Void) {
    print("Response received for \(response.actionIdentifier)")
    completionHandler()
}
```

### 第五步，自定义通知。
    

### 第六步，管理队列中的通知。
可以通过调用系统给定的特定api来对通知进行删、改的管理。通过可视化的table方式对展示到table中的通知删除，可以手动删除等待队列中的通知。
```swift
override func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCellEditingStyle, forRowAt indexPath: IndexPath) {
    //No.9 手动删除等待队列中的通知
    guard let section =
        NotificationTableSection(rawValue: indexPath.section),
        editingStyle == .delete && section == .pending else { return }
    guard let provider = tableSectionProviders[.pending]
        as? PendingNotificationsTableSectionProvider else { return }
    let request = provider.requests[indexPath.row]
    UNUserNotificationCenter.current()
        .removePendingNotificationRequests(withIdentifiers:
            [request.identifier])
    loadNotificationData(callback: {
        self.tableView.deleteRows(at: [indexPath], with: .automatic)
    })
  }
}
```

### 第七步，查看系统通知权限设置。
```swift
func loadNotificationData(callback: (() -> ())? = .none) {
    let group = DispatchGroup()
     查询系统关于通知的设置并展示到table中
    let notificationCenter = UNUserNotificationCenter.current()
    let dataSaveQueue = DispatchQueue(label: "com.raywenderlich.CuddlePix.dataSave") //防止并发性错误
    group.enter() // 确保所有查询动作都完成才更新table
    notificationCenter.getNotificationSettings { 
        (settings) in
        let settingsProvider = SettingTableSectionProvider(settings: settings, name: "Notification Settings")
        dataSaveQueue.async(
            execute: { //异步更新table
            self.tableSectionProviders[.settings] = settingsProvider
            group.leave()
        })
    }
    //加入两个sections展示Pending和Delivered的通知
    group.enter()
    notificationCenter.getPendingNotificationRequests { (requests) in
        let pendingRequestsProvider = PendingNotificationsTableSectionProvider(requests: requests, name: "Pending Notifications")
        dataSaveQueue.async(execute: { self.tableSectionProviders[.pending] = pendingRequestsProvider
        group.leave()
        })
    }
    group.enter()
    notificationCenter.getDeliveredNotifications { 
    (notifications) in
        let deliveredNotificationsProvider = DeliveredNotificationsTableSectionProvider(notifications: notifications, name: "Delivered Notifications")
        dataSaveQueue.async(execute: {
            self.tableSectionProviders[.delivered] = deliveredNotificationsProvider
            group.leave()
        })
    }
    
    group.notify(queue: DispatchQueue.main) {
        if let callback = callback {
            callback()
        } else {
        self.tableView.reloadData()
        }
    }
 
}
```

