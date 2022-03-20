# [SideMenuDemo](https://www.youtube.com/watch?v=e8OtfA3YvSM)

<img width="516" src="https://github.com/YamamotoDesu/SideMenuDemo/blob/main/Gif/simpleSideMenu.gif">

## [How to Implement SideMenu](https://github.com/jonkykong/SideMenu)
Podfile
```ruby
  pod 'SideMenu'
```

ContainerViewController
```swift
import UIKit
import SideMenu

class ContainerViewController: UIViewController, MenuControllerDelegate {
    
    private var sideMenu: SideMenuNavigationController?
    
    private let settingsController = SettingsViewController()
    private let infoControoler = InfoViewController()

    override func viewDidLoad() {
        super.viewDidLoad()
        let menu = MenuController(with: SideMenuItem.allCases)
        
        menu.delegate = self
        
        sideMenu = SideMenuNavigationController(rootViewController: menu)
        
        sideMenu?.leftSide = true
        SideMenuManager.default.leftMenuNavigationController = sideMenu
        SideMenuManager.default.addPanGestureToPresent(toView: view)
        addChildController()
    }
    
    private func addChildController() {
        addChild(settingsController)
        addChild(infoControoler)
        
        view.addSubview(settingsController.view)
        view.addSubview(infoControoler.view)
        
        settingsController.view.frame = view.bounds
        infoControoler.view.frame = view.bounds
        
        settingsController.didMove(toParent: self)
        infoControoler.didMove(toParent: self)
        
        settingsController.view.isHidden = true
        infoControoler.view.isHidden = true
    }

    @IBAction func didTapMenuButton(_ sender: UIBarButtonItem) {
        present(sideMenu!, animated: true)
        
    }
    
    func didSelectMenuItem(named: SideMenuItem) {
        sideMenu?.dismiss(animated: true, completion: nil)
        
        title = named.rawValue
        
        switch named {
        case .home:
            settingsController.view.isHidden = true
            infoControoler.view.isHidden = true
        case .info:
            settingsController.view.isHidden = true
            infoControoler.view.isHidden = false
        case .settings:
            settingsController.view.isHidden = false
            infoControoler.view.isHidden = true
        }
      
    }
    
}

```
