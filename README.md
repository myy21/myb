def save_plan(self, plan: Dict, filename: str = "spring_outing_plan.json") -> bool:
        """保存计划到文件"""
        try:
            with open(filename, 'w', encoding='utf-8') as f:
                json.dump(plan, f, ensure_ascii=False, indent=2)
            return True
        except Exception as e:
            print(f"保存计划失败: {e}")
            return False
 
def main():
    planner = SpringOutingPlanner()
    
    print("=== 春游计划助手 ===")
    print("1. 推荐春游景点")
    print("2. 查询天气")
    print("3. 规划行程")
    print("4. 保存计划")
    
    choice = input("请选择功能(1-4): ")
    
    if choice == "1":
        location = input("输入您想去的城市(可选): ")
        recommendations = planner.recommend_attractions(location)
        print("\n=== 推荐景点 ===")
        for i, attr in enumerate(recommendations, 1):
            print(f"{i}. {attr['name']} ({attr['location']}) - 评分: {attr['rating']}")
    
    elif choice == "2":
        city = input("输入城市名称: ")
        date = input("输入日期(YYYY-MM-DD，可选，直接回车为今天): ") or None
        weather = planner.get_weather(city, date)
        print("\n=== 天气信息 ===")
        if "error" in weather:
            print(weather["error"])
        else:
            print(f"城市: {weather['city']}")
            print(f"日期: {weather['date']}")
            print(f"温度: {weather['temp']}°C")
            print(f"天气: {weather['weather']}")
            print(f"湿度: {weather['humidity']}%")
            print(f"风速: {weather['wind_speed']} m/s")
    
    elif choice == "3":
        days = int(input("输入春游天数: "))
        print("\n可选景点:")
        recommendations = planner.recommend_attractions()
        for i, attr in enumerate(recommendations, 1):
            print(f"{i}. {attr['name']} ({attr['location']})")
        
        attraction_nums = input("输入想去的景点编号(用逗号分隔): ")
        attraction_indices = [int(x.strip())-1 for x in attraction_nums.split(',')]
        selected_attractions = [recommendations[i]["name"] for i in attraction_indices if i < len(recommendations)]
        
        itinerary = planner.plan_itinerary(days, selected_attractions)
        print("\n=== 行程规划 ===")
        for day, attrs in itinerary.items():
            print(f"第{day}天: {', '.join(attrs)}")
    
    elif choice == "4":
        # 这里应该先获取计划数据，简化示例直接保存
        sample_plan = {
            "date": datetime.now().strftime("%Y-%m-%d"),
            "attractions": ["颐和园", "西湖"],
            "days": 2,
            "itinerary": {
                1: ["颐和园"],
                2: ["西湖"]
            }
        }
        if planner.save_plan(sample_plan):
            print("计划已保存为 spring_outing_plan.json")
    
    else:
        print("无效选择")
 
if __name__ == "__main__":
    main()
