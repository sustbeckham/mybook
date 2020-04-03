## 观察者模式

参考:
[https://mlog.club/article/13159]

拼多多专家岗有问。

当然JDK1.0开始就提供了对应的API。Observable和Observe。前者对应这里的Subject。后者对应这里的也是Observe。

	private interface Observe{
		void notification();
	}

	private interface Subject{
		// 增加OR删除内部管理的观察者对象
		void addListener(Observe observe);
		void removeListener(Observe observe);
		// 负责通知observe更新
		void update();
	}

	private static class PlaySubject implements Subject{
		List<Observe> observes = new ArrayList<>();
		@Override
		public void addListener(Observe observe) {
			observes.add(observe);
		}

		@Override
		public void removeListener(Observe observe) {
			observes.remove(observe);
		}

		@Override
		public void update() {
			for(Observe observe : observes){
				observe.notification();
			}
		}
	}

	public static void main(String[] args) {
		PlaySubject subject = new PlaySubject();
		subject.addListener(new Observe() {
			@Override
			public void notification() {
				System.out.println("卧槽新加入了Listener");
			}
		});
		subject.addListener(new Observe() {
			@Override
			public void notification() {
				System.out.println("天哪");
			}
		});
		subject.update();
	}