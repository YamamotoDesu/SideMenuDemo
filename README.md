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

SideMenu
```swift
import UIKit

protocol MenuControllerDelegate {
    func didSelectMenuItem(named: SideMenuItem)
}

enum SideMenuItem: String, CaseIterable {
    case home = "Home"
    case info = "Info"
    case settings = "Settings"
}

class MenuController: UITableViewController {
    
    public var delegate: MenuControllerDelegate?
    
    private let menuItems: [SideMenuItem]
    private let color = UIColor(red: 33/255.0,
                                green: 33/255.0,
                                blue: 33/255.0,
                                alpha: 1)
    
    init(with menuItems: [SideMenuItem]) {
        self.menuItems = menuItems
        super.init(nibName: nil, bundle: nil)
        tableView.register(UITableViewCell.self,
                           forCellReuseIdentifier: "cell")
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        tableView.backgroundColor = color
        view.backgroundColor = color
    }
    
    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return menuItems.count
    }
    
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath)
        cell.textLabel?.text = menuItems[indexPath.row].rawValue
        cell.textLabel?.textColor = .white
        cell.contentView.backgroundColor = color
        return cell
    }
    
    override func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        tableView.deselectRow(at: indexPath, animated: true)
        let selectedItem = menuItems[indexPath.row]
        delegate?.didSelectMenuItem(named: selectedItem)
    }
}


```


