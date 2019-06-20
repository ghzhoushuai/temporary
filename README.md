# temporary
private void demoMethod3(){
        Observable<String> observable = Observable.create(new ObservableOnSubscribe<String>() {
            @Override
            public void subscribe(ObservableEmitter<String> emitter) throws Exception {
                Log.d("TAG", "subscribe:" + Thread.currentThread().getName());
                emitter.onNext("1");
                emitter.onNext("2");
                emitter.onNext("3");
            }
        });
        Consumer<String> consumer = new Consumer<String>() {
            @Override
            public void accept(String s) throws Exception {
                Log.d("TAG", "accept3:" +s+":"+ Thread.currentThread().getName());
            }
        };
        observable.subscribeOn(Schedulers.newThread())
                .subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())
                .doOnNext(new Consumer<String>() {
                    @Override
                    public void accept(String s) throws Exception {
                        Log.d("TAG", "accept1:" +s+":"+ Thread.currentThread().getName());
                    }
                })
                .observeOn(Schedulers.io())
                .doOnNext(new Consumer<String>() {
                    @Override
                    public void accept(String s) throws Exception {
                        Log.d("TAG", "accept2:" +s+":"+ Thread.currentThread().getName());
                    }
                })
                .subscribe(consumer);
    }
